## Zebra-Printer_Link-OS_Add Alerts by ^SX command
## アラート通知方法(^SX編)

<img src="https://images.pexels.com/photos/4271933/pexels-photo-4271933.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1">

[設定項目の詳細はZPL2 コマンドリファレンスを参照のこと](https://support.zebra.com/cpws/docs/zpl/SX_Command.pdf)



## ZPL： ^SX

設定することにより、任意のインターフェイスへイベント通知が可能。
</br>

### 利用可能なI/F

- Serial
- Prallel
- E-mail
- TCP
- UDP
- SNMP Trap (v2c)*
\* Link-OS 7.0以降でv3をサポート


</br>

### 通知可能なアラート

```
Accepted Values:
A = paper out
B = ribbon out
C = printhead over-temp
D = printhead under-temp
E = head open
F = power supply over-temp
G = ribbon-in warning (Direct Thermal Mode)
H = rewind full
I = cut error
J = printer paused
K = PQ job completed
L = label ready
M = head element out
N = ZBI (Zebra BASIC Interpreter) runtime error
O = ZBI (Zebra BASIC Interpreter) forced error
P = power on
Q = clean printhead
R = media low
S = ribbon low
T = replace head
U = battery low
V = RFID error (in RFID printers only)
* = all errors
Default Value: if the parameter is missing or invalid, the command is ignored
```

### 設定方法

例、全アラート項目をシリアル経由で通知する設定

```

## 設定
^XA
^SX*, A, Y, Y
^XZ


## 設定確認
~HU
*,A,Y,Y,,0

```

</br>

#### [参考] インターフェイス別の設定例

| I/F       | コマンド例                   |
| --------- | ---------------------------- |
| Serial    | ^SXA,A,Y,Y                   |
| Parallel  | ^SXA,B,Y,Y                   |
| E-Mail    | ^SXA,C,Y,Y,admin@company.com |
| TCP       | ^SXA,D,Y,Y,123.45.67.89,1234 |
| UDP       | ^SXA,E,Y,Y,123.45.67.89,1234 |
| SNMP Trap | ^SXA,F,Y,Y,255.255.255.255   |



</br>

#### デモ - 特定のエラーを通知

```
^XA
^SXA,A,Y,Y
^JUS
^XZ

~HU
A,A,Y,Y,,0

ERROR CONDITION: PAPER OUT [2024-08-06 06:51:26] [D7J203700352 (????????)]

^XA
^SXA,A,N,N
^JUS
^XZ

~HU

```

#### デモ - 全てのイベントを取得
```
^XA
^SX*,A,Y,Y
^JUS
^XZ

~HU
*,A,Y,Y,,0

^XA
^FO50,50^B3N,N,100,Y,N^FD123456^FS
^PQ2
^XZ

LABEL TAKEN:  [2024-07-17 14:36:16] [XXZKJ181100602]
LABEL READY:  [2024-07-17 14:36:17] [XXZKJ181100602]
LABEL TAKEN:  [2024-07-17 14:36:29] [XXZKJ181100602]
ALERT: PQ JOB COMPLETED [2024-07-17 14:36:30] [XXZKJ181100602]
LABEL READY:  [2024-07-17 14:36:30] [XXZKJ181100602]
LABEL TAKEN:  [2024-07-17 14:36:31] [XXZKJ181100602]
ERROR CONDITION: HEAD OPEN [2024-07-17 14:37:03] [XXZKJ181100602]
ERROR CLEARED: HEAD OPEN [2024-07-17 14:37:48] [XXZKJ181100602]
```
