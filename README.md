lhades
======

Lua 5.3 Byte-Code Disassembler

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
luac example.lua
lhades luac.out
```

Example
-------

```lua
local name = "John"

function hello_world(count)
    for i = 1, count do
        print("Hello!, " .. name)
    end
end

hello_world(3)
```

↓

```
source name "@test.lua": 0 - 0: 0 params, 3 stacks
.codes: 7
  [1] LOADK           0               0    ; R(0) := Kst(0)
  [2] CLOSURE         1               0    ; R(1) := closure(KPROTO[0])
  [3] SETTABUP        0     257       1    ; UpValue[0][RK(257)] := RK(1)
  [4] GETTABUP        1       0     257    ; R(1) := UpValue[0][RK(257)]
  [5] LOADK           2               2    ; R(2) := Kst(2)
  [6] CALL            1       2       1    ; R(1), ... ,R(1+1-2) := R(1)(R(1+1), ... ,R(1+2-1))
  [7] RETURN          0               1    ; return R(0), ... ,R(0+1-2)
.consts: 3
  [0] "John"
  [1] "hello_world"
  [2] 3
.upvalues: 1
  [0] inStack: true, index: 0    ; _ENV
.locals: 1
  [0] "name" (1 - 7)
.protos: 1
  [0] source name "@test.lua": 3 - 7: 1 params, 8 stacks
    .codes: 11
      [ 1] LOADK           1               0    ; R(1) := Kst(0)
      [ 2] MOVE            2               0    ; R(2) := R(0)
      [ 3] LOADK           3               0    ; R(3) := Kst(0)
      [ 4] FORPREP         1               5    ; R(1)-=R(1+2); pc+=5
      [ 5] GETTABUP        5       0     257    ; R(5) := UpValue[0][RK(257)]
      [ 6] LOADK           6               2    ; R(6) := Kst(2)
      [ 7] GETUPVAL        7               1    ; R(7) := UpValue[1]
      [ 8] CONCAT          6       6       7    ; R(6) := R(6).. ... ..R(7)
      [ 9] CALL            5       2       1    ; R(5), ... ,R(5+1-2) := R(5)(R(5+1), ... ,R(5+2-1))
      [10] FORLOOP         1              -6    ; R(1)+=R(1+2); if R(1) <?= R(1+1) then { pc+=-6; R(1+3)=R(1) }
      [11] RETURN          0               1    ; return R(0), ... ,R(0+1-2)
    .consts: 3
      [0] 1
      [1] "print"
      [2] "Hello!, "
    .upvalues: 2
      [0] inStack: false, index: 0    ; _ENV
      [1] inStack: true, index: 0    ; name
    .locals: 5
      [0] "count" (0 - 11)
      [1] "(for index)" (3 - 10)
      [2] "(for limit)" (3 - 10)
      [3] "(for step)" (3 - 10)
      [4] "i" (4 - 9)
    .protos: 0
```
