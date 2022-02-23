CBC-MAC is secure a long as q<< |X|<sup>1/2</sup> 

HMAC is secure as long as q << |K|<sup>1/2</sup> (2<sup>64</sup> for AES-128)

Suppose we want Adv<sub>PRF</sub>[A,F<sub>ECBC</sub>]<= 1/2<sup>32</sup>    <== q<sup>2</sup>/|X|< 1/2<sup>32</sup>

- AES: |X|=2<sup>128</sup> => q< 2<sup>48</sup> 
	So, after 2<sup>48</sup> messages must change key

- 3DES: |X|=2<sup>64</sup> => q< 2<sup>16</sup> 
