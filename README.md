lhades
======

Lua Byte-Code Disassembler

Install
-------

```sh
git clone https://github.com/tacigar/lhades.git
cd lhades
luarocks make
```

Usage
-----

```sh
lhades <byte-code filename>
```


Example
-------

```lua
local x = 7
local function f(y)
	return function()
		return x + y
	end
end
local y = f(3)

for i = 1, 5 do
	print(y())
end
```

↓

```
luac example.lua
```

↓

```
1b4c 7561 5300 1993 0d0a 1a0a 0408 0408
0878 5600 0000 0000 0000 0000 0000 2877
4001 0d40 6578 616d 706c 652e 6c75 6100
0000 0000 0000 0000 0209 0f00 0000 0100
0000 6c00 0000 8000 8000 c140 0000 a480
0001 c180 0000 01c1 0000 4181 0000 e8c0
0080 c601 4100 0002 0001 2402 8000 e441
0000 e780 fe7f 2600 8000 0500 0000 1307
0000 0000 0000 0013 0300 0000 0000 0000
1301 0000 0000 0000 0013 0500 0000 0000
0000 0406 7072 696e 7401 0000 0001 0001
0000 0000 0200 0000 0600 0000 0100 0203
0000 006c 0000 0066 0000 0126 0080 0000
0000 0001 0000 0001 0001 0000 0000 0300
0000 0500 0000 0000 0205 0000 0005 0000
0045 0080 000d 4000 0026 0000 0126 0080
0000 0000 0002 0000 0000 0001 0000 0000
0005 0000 0004 0000 0004 0000 0004 0000
0004 0000 0005 0000 0000 0000 0002 0000
0002 7802 7903 0000 0005 0000 0005 0000
0006 0000 0001 0000 0002 7900 0000 0003
0000 0001 0000 0002 780f 0000 0001 0000
0006 0000 0007 0000 0007 0000 0007 0000
0009 0000 0009 0000 0009 0000 0009 0000
000a 0000 000a 0000 000a 0000 000a 0000
0009 0000 000b 0000 0007 0000 0002 7801
0000 000f 0000 0002 6602 0000 000f 0000
0002 7905 0000 000f 0000 000c 2866 6f72
2069 6e64 6578 2908 0000 000e 0000 000c
2866 6f72 206c 696d 6974 2908 0000 000e
0000 000b 2866 6f72 2073 7465 7029 0800
0000 0e00 0000 0269 0900 0000 0d00 0000
0100 0000 055f 454e 56
```

↓

```
lhades luac.out
```

↓

```
source name "@example.lua": 0 - 0: 0 params, 9 stacks
.codes: 15
  [ 1] LOADK           0               0    ; R(0) := Kst(0)
  [ 2] CLOSURE         1               0    ; R(1) := closure(KPROTO[0])
  [ 3] MOVE            2               1    ; R(2) := R(1)
  [ 4] LOADK           3               1    ; R(3) := Kst(1)
  [ 5] CALL            2       2       2    ; R(2), ... ,R(2+2-2) := R(2)(R(2+1), ... ,R(2+2-1))
  [ 6] LOADK           3               2    ; R(3) := Kst(2)
  [ 7] LOADK           4               3    ; R(4) := Kst(3)
  [ 8] LOADK           5               2    ; R(5) := Kst(2)
  [ 9] FORPREP         3               4    ; R(3)-=R(3+2); pc+=4
  [10] GETTABUP        7       0       4    ; R(7) := UpValue[0][RK(4)]
  [11] MOVE            8               2    ; R(8) := R(2)
  [12] CALL            8       1       0    ; R(8), ... ,R(8+0-2) := R(8)(R(8+1), ... ,R(8+1-1))
  [13] CALL            7       0       1    ; R(7), ... ,R(7+1-2) := R(7)(R(7+1), ... ,R(7+0-1))
  [14] FORLOOP         3              -5    ; R(3)+=R(3+2); if R(3) <?= R(3+1) then { pc+=-5; R(3+3)=R(3) }
  [15] RETURN          0               1    ; return R(0), ... ,R(0+1-2)
.consts: 5
  [0] 7
  [1] 3
  [2] 1
  [3] 5
  [4] "print"
.upvalues: 1
  [0] inStack: true, index: 0    ; _ENV
.locals: 7
  [0] "x" (1 - 15)
  [1] "f" (2 - 15)
  [2] "y" (5 - 15)
  [3] "(for index)" (8 - 14)
  [4] "(for limit)" (8 - 14)
  [5] "(for step)" (8 - 14)
  [6] "i" (9 - 13)
.protos: 1
  [0] source name "@example.lua": 2 - 6: 1 params, 2 stacks
    .codes: 3
      [1] CLOSURE         1               0    ; R(1) := closure(KPROTO[0])
      [2] RETURN          1               2    ; return R(1), ... ,R(1+2-2)
      [3] RETURN          0               1    ; return R(0), ... ,R(0+1-2)
    .consts: 0
    .upvalues: 1
      [0] inStack: true, index: 0    ; x
    .locals: 1
      [0] "y" (0 - 3)
    .protos: 1
      [0] source name "@example.lua": 3 - 5: 0 params, 2 stacks
        .codes: 5
          [1] GETUPVAL        0               0    ; R(0) := UpValue[0]
          [2] GETUPVAL        1               1    ; R(1) := UpValue[1]
          [3] ADD             0       0       1    ; R(0) := RK(0) + RK(1)
          [4] RETURN          0               2    ; return R(0), ... ,R(0+2-2)
          [5] RETURN          0               1    ; return R(0), ... ,R(0+1-2)
        .consts: 0
        .upvalues: 2
          [0] inStack: false, index: 0    ; x
          [1] inStack: true, index: 0    ; y
        .locals: 0
        .protos: 0
```
