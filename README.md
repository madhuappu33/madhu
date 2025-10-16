;----------------------------------------------------------
; Program: Bubble Sort (Ascending & Descending)
; Processor: 8086
; Assembler: MASM / TASM
;----------------------------------------------------------

DATA SEGMENT
    ARR DB 25, 10, 15, 5, 30, 20    ; Array of numbers
    N   EQU 6                       ; Number of elements in the array
    MSG1 DB 0DH,0AH,'Ascending Order:$'
    MSG2 DB 0DH,0AH,'Descending Order:$'
DATA ENDS

CODE SEGMENT
ASSUME CS:CODE, DS:DATA

START:
    MOV AX, DATA
    MOV DS, AX

;-------------------------------------
; Sort in Ascending Order
;-------------------------------------
ASC_SORT:
    MOV CX, N               ; Outer loop counter (N elements)
OUTER1:
    DEC CX
    MOV SI, 0               ; Index pointer
    MOV BX, CX
INNER1:
    MOV AL, ARR[SI]
    CMP AL, ARR[SI+1]
    JBE NO_SWAP1            ; Jump if in correct order (Ascending)
    XCHG AL, ARR[SI+1]
    MOV ARR[SI], AL
NO_SWAP1:
    INC SI
    DEC BX
    JNZ INNER1
    LOOP OUTER1

;-------------------------------------
; Display Ascending Order
;-------------------------------------
    LEA DX, MSG1
    MOV AH, 09H
    INT 21H

    MOV CX, N
    MOV SI, 0
DISP_ASC:
    MOV AL, ARR[SI]
    CALL DISP_NUM
    INC SI
    LOOP DISP_ASC

;-------------------------------------
; Sort in Descending Order
;-------------------------------------
DESC_SORT:
    MOV CX, N
OUTER2:
    DEC CX
    MOV SI, 0
    MOV BX, CX
INNER2:
    MOV AL, ARR[SI]
    CMP AL, ARR[SI+1]
    JAE NO_SWAP2            ; Jump if already in descending order
    XCHG AL, ARR[SI+1]
    MOV ARR[SI], AL
NO_SWAP2:
    INC SI
    DEC BX
    JNZ INNER2
    LOOP OUTER2

;-------------------------------------
; Display Descending Order
;-------------------------------------
    LEA DX, MSG2
    MOV AH, 09H
    INT 21H

    MOV CX, N
    MOV SI, 0
DISP_DESC:
    MOV AL, ARR[SI]
    CALL DISP_NUM
    INC SI
    LOOP DISP_DESC

;-------------------------------------
; Exit program
;-------------------------------------
    MOV AH, 4CH
    INT 21H

;-------------------------------------
; Subroutine to Display a Number
;-------------------------------------
DISP_NUM PROC
    MOV BL, 10
    MOV AH, 0
    AAM                     ; AL = tens, AH = ones
    ADD AH, 30H
    ADD AL, 30H
    MOV DL, AL
    MOV AH, 02H
    INT 21H
    MOV DL, AH
    MOV AH, 02H
    INT 21H
    MOV DL, ' '
    MOV AH, 02H
    INT 21H
    RET
DISP_NUM ENDP

CODE ENDS
END START
