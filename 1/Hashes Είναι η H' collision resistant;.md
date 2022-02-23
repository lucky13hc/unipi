H'(x) = H1(H2(x)) όπου H1, H2 collision resistant. Είναι η H' collision resistant;

Έστω H' δεν είναι collision resistant. Τότε για δύο διαφορετικά μηνύματα x1, x2:

H'(x1) = H'(x2)
H1(H2(x1)) = H1(H2(x2))			(1)

Ποιές είναι οι περιπτώσεις να ισχύει το πάνω;

1. H2(x1) = H2(x2)
	Άτοπο διότι H2 collision resistant.

2. H2(x1) != H2(x2)
	Για απλότητα θέτω a = H2(x1), b = H2(x2)
	Σε αυτή την περίπτωση για να ισχύει η (1) πρέπει H1(a) = H1(b).
	Άτοπο διότι H1 collision resistant.
	
Άρα H' collision resistant.

--------------------------------------------------------------------------------

H'(x) = H(H(x)) όπου H collision resistant. Είναι η H' collision resistant;

Έστω H' δεν είναι collision resistant. Τότε για δύο διαφορετικά μηνύματα x1, x2:

H'(x1) = H'(x2)
H(H(x1)) = H(H(x2))			(1)

Ποιές είναι οι περιπτώσεις να ισχύει το πάνω;

1. H(x1) = H(x2)
	Άτοπο διότι H collision resistant.

2. H(x1) != H(x2)
	Για απλότητα θέτω a = H(x1), b = H(x2)
	Σε αυτή την περίπτωση για να ισχύει η (1) πρέπει H(a) = H(b).
	Άτοπο διότι H collision resistant.
	
Άρα H' collision resistant.