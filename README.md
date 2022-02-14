# Librerie 
```vhdl
library ieee;
use ieee.std_logic_1164.all;
```

# ENTITY
```vhdl
Entity FSM is 
Port(	ck,start: in std_logic;
	OP: in std_logic_vector(1 downto 0);
	Stato: out std_logic_vector(2 downto 0)
);
End FSM;
```

# ARCHITECTURE

```vhdl
Architecture beh of FSM is 
signal st: std_logic_vector(2 downto 0);

begin
	Stato <= st;
	
	process(ck)
	begin
		if clk'event and clk='0' then -- se siamo sul fronte di discesa del clock
			case st is	 
			 when "000" =>	  if start = '1' then st <= "001";
			 					else st <= "000";
			 				end if;
			   when "001" =>    if OP(1) = '0' then st <= "010"; 
			   					else st <= "011";
			   					end if;
			   when "010" => st <= "100";
			   when "011" => st <= "101";
			   when others => st <= "000";
			 
			end case;
		end if;
	end process
end beh;
```

