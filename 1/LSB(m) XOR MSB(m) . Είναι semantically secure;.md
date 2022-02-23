EXP(i) = Το πείραμα για το m_i	(ΤΟ i ΣΥΜΒΟΛΙΖΕΙ ΤΟ INDEX ΤΟΥ ΜΗΝΥΜΑΤΟΣ m)
EXP(0) = Το πείραμα για το m0
EXP(1) = Το πείραμα για το m1

Pr[EXP(0) = 1] ==> Η πιθανότητα το αποτέλεσμα από το πείραμα για το m0 να ειναι 1
Pr[EXP(1) = 1] ==> Η πιθανότητα το αποτέλεσμα από το πείραμα για το m1 να ειναι 1

- Έστω LSB(m0) = MSB(m0), άρα LSB(m0) XOR MSB(m0) = 0
	   LSB(m1) = MSB(m1), άρα LSB(m1) XOR MSB(m1) = 0
	   
E'(k, m0) = E(k, m0) || (LSB(m0) XOR MSB(m0)) = E(k, m0) || 0
E'(k, m1) = E(k, m1) || (LSB(m1) XOR MSB(m1)) = E(k, m1) || 0
	
Adv[B,E] = | Pr[EXP(0) = 1] - Pr[EXP(1) = 1] | = | 0 - 0 | = 0

- Έστω LSB(m0) = MSB(m0) ==> LSB(m0) XOR MSB(m0) = 0
	   LSB(m1) = MSB(m1) ==> LSB(m1) XOR MSB(m1) = 1
	   
E'(k, m0) = E(k, m0) || (LSB(m0) XOR MSB(m0)) = E(k, m0) || 0
E'(k, m1) = E(k, m1) || (LSB(m1) XOR MSB(m1)) = E(k, m1) || 1
	
Adv[B,E] = | Pr[EXP(0) = 1] - Pr[EXP(1) = 1] | = | 0 - 1 | = 1

Βρήκαμε έστω και ένα ζεύγος μηνυμάτων m0, m1 : έχει advantage ο επιτιθέμενος στο Ε'.
