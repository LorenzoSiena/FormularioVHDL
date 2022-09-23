# Compiti svolti
## 28/06/2022
![c_28062022](c_2806222.jpeg)
![28062022_SCHEMA](28062022_SCHEMA.jpeg)
```vhdl
library ieee;
use ieee.std_logic_1164.all;

Entity CU is
	port(
	clk,start,op,ready: in std_logic;
	stato:out integer range 0 to 3
	);
end CU;

architecture beh of CU is

signal st: integer range 0 to 3;

begin
	
	stato<=st;
	
	process(clk)
	
	begin
		if clk='0' and clk'EVENT then
			case st is
				when 0=> --idle
				if start='0' then st<=0;
					else st<=1;
					end if;
				when 1=> --read
					if ready ='0' then st<=1;
					else st<=2;
					end if;
				when 2=> --exe1 --vai in idle
					if op ='0' then st<=0
					end if;
				
				when others=> --exe2 --perde un ciclo e va in idle
					st<=0;
			end case;
		end if;	
	end process;
end beh;

```
## 14/03/2022
![c_14032022](c_14032022.jpeg)
```vhdl
BRO

```
## 24/02/2022 T1 (DATAPATH)
![c_24022022](24022022PLUS.jpeg)

```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;

Entity DPath is
	Port( 
		Din: in std_logic_vector(7 downto 0);
		OP: in std_logic;
		clk,WeA,WeR: in std_logic;
		R: out std_logic_vector(7 downto 0)
);
End DPath;
Architecture beh of DPath is
signal regA, Ris: std_logic_vector(7 downto 0);
begin

    Ris <= RegA + Din when OP = '0' else
		   RegA or Din;
		   
	process(clk)
	begin
	if clk = '0' and clk' event then
		if WeA = '1' then
			regA <= Din;
		end if;
		
		if WeR = '1' then
			R<= Ris;
		end if;
	end if;
	end process;
end beh;


```
![ORopAND](ORopAND.jpeg)
## 24/02/2022 T2 (DATAPATH)
![c_24022022BIS](c_24022022BIS.jpeg)
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all; 

Entity DPath is
Port( Din: in std_logic_vector(15 downto 0);
clk,WeA,WeB, WeR: in std_logic;
R: out std_logic_vector(15 downto 0)
);
End DPath;
Architecture beh of DPath is 
signal A,B: std_logic_vector (15 downto 0);
begin
	process(clk)
	begin
			if clk='0' and clk'event then
				if WeA='1' then 
					A<=Din;
				end if;
				if WeB='1' then 
					B<=Din;
				end if;
				if WeR='1' then
					if Din<A then
						R<=A+B;
					else R<=Din+B;
					end if;
				end if;
			end if;
	end process;
end beh;

```
## 31/01/2022 (CONTROL UNIT/DIAGRAMMA A STATI)
![c_31012022.jpeg](c_31012022.jpeg)
```vhdl
library ieee;
use ieee.std_logic_1164.all;  

Entity CU is
Port(clk, start: in std_logic;
Op,cnt: in std_logic; 
stato: out integer range 0 to 3 );
End CU;						   

Architecture beh of CU is 	

signal st: integer range 0 to 3;

begin
	
	stato<=st;	 
	
	process (clk)
	begin		
		if clk='0' and clk'EVENT then
			case st is
				when 0 => 
					if start='0' then st<=0;
					else st<=1;
					end if;
				when 1 =>
					if Op='0' then st<=2;
					else st<=3;
					end if;
				when 2 => st<=0;
				when others => 
					if cnt='0' then st<=3;
					else st<=0;
					end if;
			end case;
		end if;
	end process;
	
end beh;

```
## SPECIAL
![S1.jpeg](S1.jpeg)
![S2.jpeg](S2.jpeg)
![S3.jpeg](S3.jpeg)
![S4.jpeg](S4.jpeg)
```vhdl
BRO

```

## 15/07/2021
![c_15072021.jpeg](c_15072021.jpeg)
```vhdl
BRO

```
## 23/06/2021
![c_23062021.jpeg](c_23062021.jpeg)
```vhdl
BRO

```
## 15/3/2021
![c_15032021.jpeg](c_15032021.jpeg)
```vhdl
BRO

```
## 18/2/2021
![c_18022021.jpeg](c_18022021.jpeg)
```vhdl
BRO

```
## 26/01/2021 T1 
![26022021.png](26022021.png)
```vhdl
library ieee;
use ieee.std_logic_1164.all;

Entity CU is
Port(	stato: 	in std_logic_vector(1 downto 0);
	en,cnt: 	in std_logic;
	WA, WB, Exe, Ready: out std_logic);
End CU;
Architecture beh of CU is 
 begin
   WA <= '1' when stato = "01" else '0';
   WB <= '1' when (stato = "01" and en='1') or stato = "10" else '0';
   exe <= '1' when stato = "11" and cnt='1'  else '0';
   Ready <= '1' when stato = "00" else '0';

end beh;

```
## 26/01/2021 T2
![26022021bis.png](26022021bis.png)
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_signed.all; 

entity DPath is
Port(Din: in std_logic_vector(15 downto 0);
	 OP: in std_logic_vector(1 downto 0);
	 clk,WA, WR: in std_logic;
	 Ris: out std_logic_vector(15 downto 0)
);
end DPath; 

architecture beh of DPath is

signal RegA: std_logic_vector(15 downto 0);
begin 
	process(clk)
	begin		
		if clk'event and clk = '0' then
			if WA = '1' then RegA <= Din;
			end if;
			if WR = '1' then 
				case OP is
					when "00" => Ris <= RegA and Din;
					when "01" => Ris <= RegA or Din;
					when "10" => Ris <= RegA - Din;
					when "11" => Ris <= RegA + Din;
					when others => null;
				end case;
			end if;
		end if;
	end process;
end beh;

```
