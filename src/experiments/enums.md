# Enums

Rust
Ada/Spark
Discriminants can have inner fields


How do we do that?


Swap_Enum and Print_enum
	Description: swapping two variants. Printing variants. It was interesting to print them to make sure representation was kept and that they were found (honestly not clear whether the swapping was enough)
Enum_hello	
	Prints in Ada.

The enumerations we are testing have just discriminants and are without fields
// This is the discriminant enum.
#[repr(C)]
enum MyEnumDiscriminant { A, B, C, D }
“For field-less enums, primitive representations set the size and alignment to be the same as the primitive type of the same name. For example, a field-less enum with a u8 representation can only have discriminants between 0 and 255 inclusive.” → this means that for the type of enums I am looking at, I just need to align the size.

```rust
#[repr(u8)] // repr(C) or repr(u8)??
enum TwoEnum {
    One,
    Two,
}
```

“This is the most important repr. It has fairly simple intent: do what C does. The order, size, and alignment of fields is exactly what you would expect from C or C++. Any type you expect to pass through an FFI boundary should have repr(C), as C is the lingua-franca of the programming world. This is also necessary to soundly do more elaborate tricks with data layout such as reinterpreting values as a different type.”
https://doc.rust-lang.org/nomicon/other-reprs.html?highlight=enum#reprc
“repr(C) is equivalent to one of repr(u*) (see the next section) for fieldless enums. The chosen size is the default enum size for the target platform's C application binary interface (ABI). Note that enum representation in C is implementation defined, so this is really a "best guess". In particular, this may be incorrect when the C code of interest is compiled with certain flags.”
“The term "fieldless enum" only means that the enum doesn't have data in any of its variants. A fieldless enum without a repr(u*) or repr(C) is still a Rust native type, and does not have a stable ABI representation. Adding a repr causes it to be treated exactly like the specified integer size for ABI purposes”
Are  #[repr(C)] and #[no_mangle] equivalent and why? 
Disassembling with `objdump`. Structure: callee is a Rust function that prints the two elements of an enum. Caller is an ada function with two simple functions. The issue that we may have identified is that if the Rust function is inlined by the gnat compiler, the object might not be found on the stack.
Disassembled function main with `objdump -d main`. 

Results:
We can see that main is calling print_enum. And print_enum is making room (0x68) on the stack (rsp register).

```asm
000000000040954c <_ada_main>:
  40954c:	55                   	push   %rbp
  40954d:	48 89 e5             	mov    %rsp,%rbp
  409550:	be 01 00 00 00       	mov    $0x1,%esi
  409555:	bf 00 00 00 00       	mov    $0x0,%edi
  40955a:	e8 11 00 00 00       	call   409570 <print_enum>
  40955f:	90                   	nop
  409560:	5d                   	pop    %rbp
  409561:	c3                   	ret
  409562:	66 90                	xchg   %ax,%ax
  409564:	66 2e 0f 1f 84 00 00 	cs nopw 0x0(%rax,%rax,1)
  40956b:	00 00 00 
  40956e:	66 90                	xchg   %ax,%ax

0000000000409570 <print_enum>:
  409570:	48 83 ec 68          	sub    $0x68,%rsp
  409574:	89 7c 24 10          	mov    %edi,0x10(%rsp)
  409578:	89 74 24 14          	mov    %esi,0x14(%rsp)
  40957c:	48 8d 7c 24 10       	lea    0x10(%rsp),%rdi
  409581:	e8 fa a2 0b 00       	call   4c3880 <_ZN4core3fmt10ArgumentV19new_debug17he6d08431bc98536dE>
  409586:	48 89 04 24          	mov    %rax,(%rsp)
  40958a:	48 89 54 24 08       	mov    %rdx,0x8(%rsp)
  40958f:	48 8d 7c 24 14       	lea    0x14(%rsp),%rdi
  409594:	e8 e7 a2 0b 00       	call   4c3880 <_ZN4core3fmt10ArgumentV19new_debug17he6d08431bc98536dE>
  409599:	48 8b 34 24          	mov    (%rsp),%rsi
  40959d:	48 8b 4c 24 08       	mov    0x8(%rsp),%rcx
  4095a2:	48 89 74 24 48       	mov    %rsi,0x48(%rsp)
  4095a7:	48 89 4c 24 50       	mov    %rcx,0x50(%rsp)
  4095ac:	48 89 44 24 58       	mov    %rax,0x58(%rsp)
  4095b1:	48 89 54 24 60       	mov    %rdx,0x60(%rsp)
  4095b6:	48 8d 7c 24 18       	lea    0x18(%rsp),%rdi
  4095bb:	48 8d 35 6e 9e 3e 00 	lea    0x3e9e6e(%rip),%rsi        # 7f3430 <__do_global_dtors_aux_fini_array_entry+0x8>
  4095c2:	ba 03 00 00 00       	mov    $0x3,%edx
  4095c7:	48 8d 4c 24 48       	lea    0x48(%rsp),%rcx
  4095cc:	41 b8 02 00 00 00    	mov    $0x2,%r8d
  4095d2:	e8 e9 a2 0b 00       	call   4c38c0 <_ZN4core3fmt9Arguments6new_v117h7811b6f7533a8fcfE>
  4095d7:	48 8d 7c 24 18       	lea    0x18(%rsp),%rdi
  4095dc:	ff 15 1e 60 3f 00    	call   *0x3f601e(%rip)        # 7ff600 <_DYNAMIC+0x750>
  4095e2:	48 83 c4 68          	add    $0x68,%rsp
  4095e6:	c3                   	ret
  4095e7:	66 0f 1f 84 00 00 00 	nopw   0x0(%rax,%rax,1)
  4095ee:	00 00 
```

Redoing the experiment without the standard library to reduce size of compiled object

Grepping symbols One and Two:

`objdump -d main | grep One`
```assembly
0000000000453bc0 <_ZN49_$LT$i8$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hc8026bc8ff200905E>:
0000000000453bd0 <_ZN50_$LT$i16$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h42da5a358db0b534E>:
0000000000453be0 <_ZN50_$LT$i32$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hf774510fbada8001E>:
0000000000453bf0 <_ZN50_$LT$i64$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h5efae2a7dcd51643E>:
```

`objdump -d main | grep Two`
```asm
00000000004095f0 <_ZN48_$LT$lib..TwoNum$u20$as$u20$core..fmt..Debug$GT$3fmt17hce2371c29cea3f48E>:
  4095ff:       75 1f                   jne    409620 <_ZN48_$LT$lib..TwoNum$u20$as$u20$core..fmt..Debug$GT$3fmt17hce2371c29cea3f48E+0x30>
  40961e:       eb 1d                   jmp    40963d <_ZN48_$LT$lib..TwoNum$u20$as$u20$core..fmt..Debug$GT$3fmt17hce2371c29cea3f48E+0x4d>
0000000000470d20 <_ZN67_$LT$memchr..memmem..twoway..TwoWay$u20$as$u20$core..fmt..Debug$GT$3fmt17h71d67439ac0ca6daE>:
00000000004b98d0 <_ZN71_$LT$core..str..pattern..TwoWaySearcher$u20$as$u20$core..fmt..Debug$GT$3fmt17hf9901cf979917cc3E>:
  4c3884:       48 8d 05 65 5d f4 ff    lea    -0xba29b(%rip),%rax        # 4095f0 <_ZN48_$LT$lib..TwoNum$u20$as$u20$core..fmt..Debug$GT$3fmt17hce2371c29cea3f48E>
```

`objdump -d mylib.a | grep One`
```asm
Disassembly of section .text._ZN49_$LT$i8$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hc8026bc8ff200905E:
0000000000000000 <_ZN49_$LT$i8$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hc8026bc8ff200905E>:
Disassembly of section .text._ZN50_$LT$i16$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h42da5a358db0b534E:
0000000000000000 <_ZN50_$LT$i16$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h42da5a358db0b534E>:
Disassembly of section .text._ZN50_$LT$i32$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hf774510fbada8001E:
0000000000000000 <_ZN50_$LT$i32$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17hf774510fbada8001E>:
Disassembly of section .text._ZN50_$LT$i64$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h5efae2a7dcd51643E:
0000000000000000 <_ZN50_$LT$i64$u20$as$u20$std..sys..unix..IsMinusOne$GT$12is_minus_one17h5efae2a7dcd51643E>:
```
When doing the other way around, from Rust to Ada:
```asm
00000000000074b0 <main>:
    74b0:	50                   	push   %rax
    74b1:	48 89 f2             	mov    %rsi,%rdx
    74b4:	48 8d 05 cf 96 03 00 	lea    0x396cf(%rip),%rax        # 40b8a <__rustc_debug_gdb_scripts_section__>
    74bb:	8a 00                	mov    (%rax),%al
    74bd:	48 63 f7             	movslq %edi,%rsi
    74c0:	48 8d 3d c9 ff ff ff 	lea    -0x37(%rip),%rdi        # 7490 <_ZN10enum_hello4main17habdaf99ca92dbd60E>
    74c7:	31 c9                	xor    %ecx,%ecx
    74c9:	e8 42 ff ff ff       	call   7410 <_ZN3std2rt10lang_start17h8d013fdbec3b61faE>
    74ce:	59                   	pop    %rcx
    74cf:	c3                   	ret
```
`objdump -d target/debug/enum_hello | grep -A20` 'enum_hello'

```asm

target/debug/enum_hello:     file format elf64-x86-64


Disassembly of section .init:

0000000000004ff0 <_init>:
    4ff0:       f3 0f 1e fa             endbr64
    4ff4:       48 83 ec 08             sub    $0x8,%rsp
    4ff8:       48 8b 05 f1 ac 3f 00    mov    0x3facf1(%rip),%rax        # 3ffcf0 <__gmon_start__@Base>
    4fff:       48 85 c0                test   %rax,%rax
    5002:       74 02                   je     5006 <_init+0x16>
    5004:       ff d0                   call   *%rax
    5006:       48 83 c4 08             add    $0x8,%rsp
    500a:       c3                      ret

Disassembly of section .plt:

0000000000005010 <__xstat64@plt-0x10>:
    5010:       ff 35 3a a9 3f 00       push   0x3fa93a(%rip)        # 3ff950 <_GLOBAL_OFFSET_TABLE_+0x8>
    5016:       ff 25 3c a9 3f 00       jmp    *0x3fa93c(%rip)        # 3ff958 <_GLOBAL_OFFSET_TABLE_+0x10>
    501c:       0f 1f 40 00             nopl   0x0(%rax)
--
0000000000007490 <_ZN10enum_hello4main17habdaf99ca92dbd60E>:
    7490:       50                      push   %rax
    7491:       c7 04 24 00 00 00 00    movl   $0x0,(%rsp)
    7498:       c7 44 24 04 01 00 00    movl   $0x1,0x4(%rsp)
    749f:       00 
    74a0:       31 ff                   xor    %edi,%edi
    74a2:       be 01 00 00 00          mov    $0x1,%esi
    74a7:       ff 15 7b 8a 3f 00       call   *0x3f8a7b(%rip)        # 3fff28 <enum_hello@SYMS>
    74ad:       58                      pop    %rax
    74ae:       c3                      ret
    74af:       90                      nop

00000000000074b0 <main>:
    74b0:       50                      push   %rax
    74b1:       48 89 f2                mov    %rsi,%rdx
    74b4:       48 8d 05 cf 96 03 00    lea    0x396cf(%rip),%rax        # 40b8a <__rustc_debug_gdb_scripts_section__>
    74bb:       8a 00                   mov    (%rax),%al
    74bd:       48 63 f7                movslq %edi,%rsi
    74c0:       48 8d 3d c9 ff ff ff    lea    -0x37(%rip),%rdi        # 7490 <_ZN10enum_hello4main17habdaf99ca92dbd60E>
    74c7:       31 c9                   xor    %ecx,%ecx
    74c9:       e8 42 ff ff ff          call   7410 <_ZN3std2rt10lang_start17h8d013fdbec3b61faE>
    74ce:       59                      pop    %rcx
    74cf:       c3                      ret

00000000000074d0 <__rust_alloc>:
    74d0:       e9 0b 78 01 00          jmp    1ece0 <__rdl_alloc>
    74d5:       66 2e 0f 1f 84 00 00    cs nopw 0x0(%rax,%rax,1)
    74dc:       00 00 00 
    74df:       90                      nop

00000000000074e0 <__rust_dealloc>:
    74e0:       e9 4b 78 01 00          jmp    1ed30 <__rdl_dealloc>
    74e5:       66 2e 0f 1f 84 00 00    cs nopw 0x0(%rax,%rax,1)
    74ec:       00 00 00 
    74ef:       90                      nop

00000000000074f0 <__rust_realloc>:
    74f0:       e9 4b 78 01 00          jmp    1ed40 <__rdl_realloc>
    74f5:       66 2e 0f 1f 84 00 00    cs nopw 0x0(%rax,%rax,1)
```



