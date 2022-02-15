# Librerie 
```vhdl
--Questo è un commento
library ieee;
use ieee.std_logic_1164.all;
```

# ENTITY
Interfaccia con le porte (IN,OUT,INOUT,BUFFER)
```vhdl
Entity NOME_ENTITA is 
Port(	ck,start: in std_logic;
	OP: in std_logic_vector(1 downto 0);
	Stato: out std_logic_vector(2 downto 0)
);
End NOME_ENTITA;
```

# ARCHITECTURE (Behavioural)

```vhdl
Architecture beh of NOME_ENTITA is 
-- SEZIONE DICHIARATIVA


begin

-- SEZIONE ESECUTIVA

end beh;
```

# Process  
Funzione che si attiva quando uno dei segnali della sensitivity list (a,b,c) cambia [EVENTO!]


```vhdl
ENTITY andor IS
Port ( a,b,c: IN std_logic; -- SENSITIVITY LIST
       a2, o2: OUT std_logic);
END andor;
ARCHITECTURE seq OF andOR IS
-- SEGNALI E VARIABILI
BEGIN
	PROCESS(a, b,c)		 -- [a2 ,o2]-> NON CAMBIANO!
		BEGIN		 -- [a2 ,o2]-> NON CAMBIANO!
		a2 <= a AND b;	 -- [a2 ,o2]-> NON CAMBIANO!
		o2<= a OR c;	 -- [a2 ,o2]-> NON CAMBIANO!
	END PROCESS;             -- [a2 ,o2]-> CAMBIANO ADESSO!
END seq;
```

### Update Segnali
Le assegnazioni dei segnali avvengono DOPO il process quindi alla fine dell'intera funzione
```vhdl
signal t: std_logic;           -- Dichiaro il segnale ausiliario t
```

### Update Variabili
Per questo uso e dichiaro dentro il process delle variabili locali che cambiano immediatamente valore.

```vhdl
variable t: std_logic        -- Dichiaro la variabile ausiliaria t
```
# IF->THEN ELSIF->THEN ELSE->THEN ENDIF ( (N-1?) MULTIPLEXER IN CASCATA)
```vhdl
process (a,b,c,s)

begin
if (s = '1') then q<= a;
elsif (s = '0') then q<= b;
else q <= b;
end if;

end process;
```
# CASE (MULTIPLEXER N ENTRATE)

```vhdl
CASE espressione IS
	WHEN valore => istruz;
	WHEN valore2a | valore2b => istruz;
	WHEN valore3 to valoreN  => istruz;
	WHEN valore4 => istruz;
	WHEN OTHERS => istruz; -- default case
END CASE;
```
# while (⚠️) ➿

# for (⚠️) ➿

# DECODER 2x4
Seleziono un uscita q(0,1,2,3) attraverso un entrata binaria i(00,01,10,11) 
```vhdl

entity decoder2x4 is
port ( i: in std_logic_vector(1 downto 0);
en: in std_logic;
q: out std_logic_vector( 0 to 3)
);
end decoder2x4;


architecture dataflow of decoder2x4 is
begin
q(0) <= ‘1’ when en =‘1’ AND i= “00” else
‘0’;
q(1) <= ‘1’ when en =‘1’ AND i= “01” else
‘0’;
q(2) <= ‘1’ when en =‘1’ AND i= “10” else
‘0’;
q(3) <= ‘1’ when en =‘1’ AND i= “11” else
‘0’;
end dataflow;

-- OPPURE 

architecture beh of decoder2x4 is
begin
q <= "1000" when en ='1' AND i= "00" else
"0100" when en ='1' AND i= "01" else
"0010" when en ='1' AND i= "10" else
"0001" when en ='1' AND i= "11" else
"0000";
end beh;

```
# FLIP-FLOP DATA (FFD) con RESET
L'entrata diventa l'uscita nella salita del clock
![FFD](FFDRES.png)

```vhdl
ENTITY ffd IS
PORT ( rst,d, clk: IN std_logic;
	q: OUT std_logic );
END ffd;

ARCHITECTURE behavioral OF ffd IS
BEGIN
	PROCESS(CLK,RST)
	BEGIN
		IF RST=‘1’ THEN q <= ‘0’ ;  -- SE PREMO RESET -> USCITA 0
		ELSIF clk=‘0’ AND clk’EVENT -- SE IL CLOCK PASSA DA 0 A 1
		THEN q<= d AFTER 2ns; 	    -- DOPO 2 NANOSEC DATA-> USCITA
		END IF;
	END PROCESS;
END behavioral;
```
# ALU (ADD,SUB,AND,OR) [IF]
```vhdl
entity alu is
	port (
	a,b: in std_logic_vector(3 downto 0);
	op: in integer rango 0 to 3;
	ris: out std_logic_vector(3 downto 0));
end alu;

architecture seq of alu is
begin
	process(a,b,op)
	begin
		if op = 0    then ris <= a and b;
		elsif op = 1 then ris <= a or b;
		elsif op = 2 then ris <= a+b;
		else ris <= a-b;
		end if;
	end process;
end seq;
```

# ALU (ADD,SUB,AND,OR) [CASE]

```vhdl
entity alu is
port ( 
	a, b: in std_logic_vector( 3 downto 0);
	op: in integer range 0 to 3;
	ris: out std_logic_vector( 3 downto 0));
end alu;

architecture seq of alu is
begin
	process(a,b,op)
	begin
		case op is
		when 0 => ris <= a and b;
		when 1 => ris <= a or b;
		when 2 => ris <= a + b;
		when others=> ris <= a - b;
		end case;
	end process;
end seq;
```


# TESTBENCH ESEMPIO (⚠️)

```vhdl
library ieee;
use ieee.std_logic_1164.all;
entity tb is
end tb;
architecture ttb of tb is
component decoder2x4 is
port ( i: in std_logic_vector(1 downto 0);
en: in std_logic;
q: out std_logic_vector( 0 to 3)
);
end component;
signal i: std_logic_vector(1 downto 0);
signal en: std_logic;
signal q: std_logic_vector( 0 to 3);

begin
dut: decoder2x4 port map (i,en,q);
en <= '0' after 0 ns, '1' after 10 ns;
i <= "00" after 0 ns, "01" after 20 ns, "10" after 30
ns, "11" after 40 ns;
end ttb;
```
