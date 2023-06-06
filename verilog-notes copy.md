# Verilog HDL

<details>
<summary><h1>Hardware description language </h1></summary>

* Similar to programming langauges but specifically oriented for describing hardware structures and behavious.
* Main diffrence: HDl's represent extensive parallel operation whereas other languages execute in a serial manner
* Allow the design of complex deigital systems with diffret levels of abstraction
* Two ways to describe the hadware: Structural Description and Dataflow Description

<details open>
<summary> <h2>Structural Description</h2></summary>

* A textual descriptionof circuit schematics
* List all of the components used and their interconnection- also know as Netlist
* Components can be either primitive logic gates (and,or,not,etc) or more complex function blocks with defined inputs and outputs.

</details>

<details open>
<summary> <h2>Dataflow Description</h2></summary>

*  functional description of the circuit
*  Can be represnt Boolean equations, truth tables, and complex operations such as arthemetic
*  Ability for decesion making and timing specification.
*  The designer does not need to know the actual implementation details (although a sensible use of the language is adviced)

</details>
</details>

<details open>
<summary><h1>Keywods and Identifiers</h1></summary>

* Verilog defines some reserved keywords for program control
* In the previous example the keywords are 
  * module,input,output,wire,not,and,endmodule.

* User-defiend idetifiers begin with a letter or underscore and can contain letters,digits,underscores and the dollar sign
* Keywords and identifiers are alll cAsE sEnSiTiVe.

</details>

<details open>
<summary><h1>Module Declaration.</hq></summary>

```verilog
module module-name (port-name,port-name,port-name ....,port-name);

        Input declarations
        Output declarations
        Net declarations
        Variable declarations
        Parameter declarations

        concurrent statments
endmodule
```

* Defines the name and I/O ports of the module
* Encloses the statments that make up the module
* A module can be represented graphically using a function block.
</details>

<details open>
<summary ><h1>Input Output Declaration.</h1></summary>

```verilog
input identifier,identifier, ...,identifier;
output identifier, identifier, ...,identifier;

input [msb:lsb] idnetifier,identifier, ...,identifier;
output [msb:lsb] identifier,identifier, ...,identifier;
```
* For each siginal of teh ports defined in **module** define the siginal direction
* Input siginals can only be read
* Output siginals can only be written to (unless defined as **reg** variables).
</details>


<details open>
<summary><h1>Net declarations.</h1></summary>

```verilog
wire identifier, identidier, ...,identiier;
wire [msb:lsb] identifier,identifier, ...,identifier;
```
* Nets are similar to physical wire an dprovide connectivity between structural elements.
* The default and mostly used net type is a **wire**
* I/O siginal types are wires by default
* Any other internal nets must be declared.
</details>
<details open>
<summary><h1>Concurrent statments - Components</h1></summary>

```verilog
component-name instatnce-identifier(expr,expr, ...,expr);
```
* Instantiate a component, define the instance name nsd connect siginals to the respective ports.
* Component can be other Verilog modules,library modules or built-in gate types.

</details>

<details open>
<summary><h1>Vectors</h1></summary>

* Siginals can be grouped together in a vector.
* The Vector defination syntax is:
    [msb:lsb]
* **msb** is the index number of the leftmost bit and **lsb** is teh index of the rightmost one.
* Index numbers can be ascending or decending.

</details>
<details open>
<summary><h1>Built-in Gates</h1></summary>

```verilog
and     xor
nand    xnor
or      buf
nor     not
```
* All of these gate types have defined ports order:
    **(output,input, input, ...)**
</details>

# Structural Verilog 

## Example 1:
```verilog
// 2-to-4-line Decoder with Enable: Structural Verilog Description.

module decoder_2_to_4_st_v(EN,A0,A1,D0,D1,D2,D3);
    input EN,A0,A1;
    output D0,D1,D2,D3;

    wire A0_n,A1_n,N0,N1,N2,N3;

    not gn0(A0_n,A0);
    not gn1(A1_n,A1);

    and gn2(N0,A0_n,A1_n);
    and gn3(N1,A0,A1_n);
    and gn4(N2,A0,A1_n);
    and gn5(N3,A0,A1);
    and gn6(D0,N0,EN);
    and gn7(D1,N1,EN);
    and gn8(D2,N2,EN);
    and gn9(D3,N3,EN);

endmodule
```
## Example 2:
```verilog
// 4-to-1-line Multiplexer: Structural Verilog Description.

module multipler_4_to_1_st_v(S,I,Y);
    input [1:0] S;
    input [3:0] I;
    output Y;

    wire [1:0] not_S;
    wire[0:3] D,N;

    not gn0(not_S[0], S[0]);
    not gn1(not_S[1], S[1]);

    and ga3(D[0],not_S[1],not_S[0]);
    and ga3(D[1],not_S[1],S[0]);
    and ga4(D[2], S[1],not_S[0]);
    and ga5(D[3],S[1],S[0]);
    and ga6(N[0],D[0],I[0]);
    and ga7(N[1],D[1],I[1]);
    and ga8(N[2],D[2],I[2]);
    and ga9(N[3],D[3],I[3]);

    or ga10(Y, N[0],N[1],N[2],N[3]);

endmodule
```
# Dataflow Verilog
<details open>
<summary><h1>Verilog Logic System </h1></summary>

* Four valued logic system:

        0 Logical 0
        1 Logical 1
        X As unknown logical value
        Z High-Impedance
</details>
<details open>
<summary><h1>Bitwise-Boolean Operators</h1></summary>

    Operator    Operation
    ~           Bitwise NOT
    &           Bitwise AND
    |           Bitwise OR
    ^           Bitwise XOR
    ^~ or ~^    Bitwise XNOR

  * Bitwise operation between single-bit values or multiple-bit vectors.

</details>

<details open>
<summary><h1>Continuous Assigment</h1></summary>

```verilog
assign net-name = expression;
assign net-name[bit-index] = expression;
assign ney-name[msb:lsb] = expression;
```
* Continuously evaluate the value on the right hand side and assign it to the left hand side
* All continuous assigments in a module evaluate concurrently.
</details>

## Example 3:
```verilog
// 2-t0-4-line Decoder with Enable: Dataflow Verilog.

module decoder_2_to_4_df_v(EN,A0,A1,D0,D1,D2,D3);
    input EN,A0,A1;
    output D0,D1,D2,D3;

    assign D0 = EN & ~A1 & ~A0;
    assign D1 = EN & ~A1 & A0;
    assign D2 = EN & A1 & ~A0;
    assign D3 = EN & A1 & A0;

endmodule
```
## Example 4:
```verilog
// 4-to-1-line Multiplexer: Data flow Verilog

module multiplexer_4_to_1_df_v(S,I,Y);
    input [1:0] S;
    input [3:0] I;

    output Y;

    assign Y = (~S[1] & ~S[0] & I[0])| (~S[1] & S[0] & I[1]) | (S[1 & ~S[0]] & I[2]) | (S[1] & S[0] & I[3])

endmodule
```

<details open>
<summary><h1>Numeric Literals</h1></summary>

* Numbers with no specific format are interpreted as decimals (usally 32 bits)
* May specify a base and number of bits using the format:   
  
    n'Bdddd

* n is the size in bits
* B is the base- one of :
  
        b    binary
        o    octal
        h    hexadecimal
        d    decimal

* ddd...d is string of digits in the specific base.
</details>

<details open>
<summary><h1> Arthemetic and Shift Operators</h1></summary>

    Operators   Operation
          +     Addition
          -     Subraction
          *     Multiplication
          /     Division
          %     Modulus (remandier)
          <<    Shift left
          >>    Shift right

* Addition and Subraction most often used.
* Multiplicationare complex to syntesize.
* Division and Modulous are generally impossible to synthesize.
</details>
<details open>
<summary><h1>Conditional Operators</h1></summary>

```
logical-expression ? true-expression : false-expression
```
* Returns the value of one of teh expressions,and depending on th e outcome of teh condition.
* Commonly nested for complex expression; use of paranthese is recommended.
* Example:
```
Z = (A > B) ? A : B;
```
</details>

## Example 5:
```verilog
// 4-to-1 line Multipler: Dataflow verilog
module multipler_4_to_1_df_v(S,I,Y);
    input [1:0] S;
    input [3:0] I;

    output Y;

    assign Y = (S == 2'b00) ? I[0] :
               (S == 2'b01) ? I[1] :
               (S == 2'b10) ? I[2] :
               (S == 2'b11) ? I[3] : 1'bX;

endmodule
```

<details open>
<summary><h1> True and  False Values </h1></summary>

* For logiv=cal operations, Verilog defines the True/False values as:
* Any **non-zero** value (1-bit or multi-bit)is condsider to be **true**
* Only the value **zero**(1-bit or multi-bit) is considered to be **false**

</details>
<details open>
<summary><h1> Logical Operations </h1></summary>

    Operator    Operation
    &&          Logical AND
    ||          Logical OR
    !           Logical NOT
    ==          Logical equality
    !=          Logical inequality
    >           Greater than
    >=          Gretaer than or equal
    <           Less than
    <=          Less than or equal

* Logical operators work on the logical values True/False
* A **True** result yields a value of **1'b1** and **False** yields **1'b0**.
</details>

## Example 6:
```verilog
// 4-to-1 line Multiplexrer: Dataflow Verilog

module multiplxer_4_to_1_df_v(S,I,Y):
    input [1:0] S;
    input [3:0] I;

    output Y;

    assign Y = S[1] ? (S[0] ? I[3] : I[2]):
                      (S[0] ? I[1] : I[0]);
    endmodule
```

# Procedural Design

```verilog
// Positive Edge-trigger D FF with reset
// Verilog Process Description

module dff_v(CLK,RESET,D,Q):
    input CLK, RESET, D;
    output Q;
    reg Q;

    always @(posedge CLK or posedge RESET) begin
        if (RESET)
            Q <= 0;
        else
            Q <= D;
    end
endmodule
```
<details open>
<summary><h1>Always Block</h1></summary>

```verilog
always @(signal-name or signal-name or ... or signal-name)
    procedural-statment

always @(posedge signal-name or needge signal-name or ...)
    procedural-statment

```
* Defines a process block that excecutes sequentially on the events given in th e sensitivity list
* Execution may be triggred on any signl change, on the positive edge of a signal or on the negative edge of a signal.
</details>

<details open>
<summary><h1>Procedural Statements</h1></summary>

* A procedural-statment may be either:
* One statment that is only valid inside a process
* A number of such statments enclosed in a **begin-end** block

```verilog
begin
    procedural-statment
    ...
    procedural-statment
end
```
</details>

<details open>
<summary><h1>Variabled</h1></summary>

```verilog
reg identifier, identifier, ...,identifier;
reg[msb:lsb] identifier, identifeir, ...,identifeir;
```

* A **reg** variable is used to store values in pricedural code
* It is not (neccessarily) a physical register or a flip-flop in the synthesized circuit.

</details>

<details open>
<summary><h1>Assigment Statments</h1></summary>

```verilog
variable-name = expression; // blocking assigment
variable-name <= expression; // non-blocking assigment
```


* Assigment may be blocking(=) or non-blocking (<=)
* A blocking assigment is executed before continuing to the following statment.
* A non-blocking assigment , all right hand sides are evaluated but only begins assgned at the end of the process.

Example of Assigment:

```verilog
// Blocking
begin
    B = A;
    C = B;
end

// Nonblocking
begin
    B <= A;
    C <= B;
end
```
<h2> Assigment Rules </h2>

* use **blocking** assigments for **combinational** logic
* Use **nonblocking** assigments for **sequential** logic
* Dont mix the the two in the same always block
* Dont assign the same variable in the two diffrent always block.
  
</details>
<details open>
<summary><h1>Prcedural Statments</h1></summary>

<h2>If Statment</h2>

```verilog 
if (condition) procedureal-statment

if (condition) procedureal-statment
else procedural statment
```

* A conditionis tested and if true , a procedural statement is executed.
* If the condition is falsse, the statement after the **else** is executed.

<h2>Case Statment </h2>

```verilog
case (selection-expression)
    choice, ..., choice: procedural-statment
    ...
    choice, ...,choice: procedural-statment
    default: procedural-statement
endcase
```

* Finds the first choice that matches the selection-expression and execute the corresponding statment.
* The **default** choice is executed if no other macthing choices were found.

<h2>Parameter Declarations</h2>

```verilog
parameter identifier = value;

parameter identifier = value,
          identifier = value,
          ...
          identifier = value;
```
* Parameters are used to define named constants.
* Improve program redability and maintainablity.

</details>

## Example 7:
```verilog
// Sequence Recognozer
// Verilog Process Description.

module seq_rec_v(CLK,RESET,X,Z):
    input  CLK, RESET, X;
    output Z;
    reg [1:0] state,next_state:
    parameter A = 2'b00,
              B = 2'b01,
              C = 2'b10,
              D = 2'b11;
    reg Z;

    // state register: implement positive
    // edge-triggered state storage with
    // asynchronus reset.

    always @(posedge CLK or posedge RESET)
    begin
        if (RESET == 1)
            state <= A;
        else
            state <= next_state;
        end
    
    // next state function: implement
    // next state state as function of X and state
    always @(X or state)
    begin
        case (state)
            A: next_state = X ? B : A;
            B: next_state = X ? C : A;
            C: next_state = X ? C : D;
            D: next_state = X ? B : A;
        endcase
    end

    // output function: implements output
    // as function of X and state

    always @(X or state)
        begin
            case (state)
                A: Z = 0; 
                B: Z = 0; 
                C: Z = 0; 
                D: Z = X ? 1 : 0; 
                //D: Z = X;
            endcase
        end
endmodule
```