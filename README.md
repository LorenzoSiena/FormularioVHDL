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

# TESTBENCH ESEMPIO (:warning:)

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
```

```vhdl
begin
dut: decoder2x4 port map (i,en,q);
en <= '0' after 0 ns, '1' after 10 ns;
i <= "00" after 0 ns, "01" after 20 ns, "10" after 30
ns, "11" after 40 ns;
end ttb;
```
