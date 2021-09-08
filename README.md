--Código creado para separar los digitos (decena y unidad) de 
--un resultado en 8 bits por medio de una conversión BCD para luego
--enviar sus digitos por separados al bloque del "siete Segmentos"

LIBRARY IEEE;
USE IEEE.STD_LOGIC_1164.ALL;
USE IEEE.STD_LOGIC_UNSIGNED.ALL;
use IEEE.numeric_std.all;

ENTITY BCD IS
  PORT(    
    todis   : IN  STD_LOGIC_VECTOR(7 DOWNTO 0);--Resultado de la operacion seleccionada en el selector
    decena  : OUT STD_LOGIC_VECTOR(3 DOWNTO 0);--Parte de las decenas 
    unidad  : OUT STD_LOGIC_VECTOR(3 DOWNTO 0));--Parte de las unidades
END ENTITY;

 ARCHITECTURE funcional OF bcd IS
 BEGIN

    process (todis)
    VARIABLE todisint : INTEGER RANGE 0 TO 81;	-- Representación en decimal del resultado que ser irá 
						-- modificando hasta llegar al valor de la unidad
    variable decenaint : INTEGER range 0 to 9; 	-- Variable que guardará el valor del dígito 'decena'
   
    BEGIN

  
    todisint := to_integer(signed(todis));	-- Este comando convierte el resultado de 8 bits en entero decimal
						-- y lo guarda en la variable previamente creada
   
    IF todisint > 79 THEN				-- La lógica para encontrar los digitos es preguntar si el resultado
       decenaint := 8;				-- en decimal está por encima de determinada decena, es decir, buscará
		 todisint := todisint - 80; 			-- si está por ejemplo en el rango 70-79 o 20-29. De ser así guardará
    ELSIF todisint > 69 THEN			-- el valor de la decena en la variable correspondiente y restará dicha
      decenaint := 7;
		-- cantidad a la variable que guarda el resultado para que este tome el
      todisint := todisint - 70;			-- valor de la Unidad
    ELSIF todisint > 59 THEN
      decenaint := 6;
      todisint := todisint - 60;
    ELSIF todisint > 49 THEN
      decenaint := 5;
      todisint := todisint - 50;
    ELSIF todisint > 39 THEN
      decenaint := 4;
      todisint := todisint - 40;
    ELSIF todisint > 29 THEN
      decenaint := 3;
      todisint := todisint - 30;
    ELSIF todisint > 19 THEN
      decenaint := 2;
      todisint := todisint - 20;
    ELSIF todisint > 9 THEN
      decenaint := 1;
      todisint := todisint - 10;  
    ELSE
      decenaint := 0;
		decena <= "0000";
    END IF;
	 decena    <= std_logic_vector(to_unsigned(decenaint,4));	-- como salidas de este bloque
    unidad    <= std_logic_vector(to_unsigned(todisint,4));	-- como salidas de este bloque
  END PROCESS bcd1;
END ARCHITECTURE funcional ;
