## One time pad Shannon
Συμπεράσματα:
- για να το πετύχεις θα πρέπει το  μήνυμα να είναι μικρότερο ή ίσο σε μήκος από το κλειδί
- Το κλειδί που χρησιμοποιείς θα πρέπει να χρησιμοποιηθεί μόνο γι' αυτό το μήνυμα ίδιου μήκους

Δεν μπορούμε να το πετύχουμε γι αυτό και έχουμε Stream Ciphers.

**QUIZ**
1. Can a stream cipher have perfect secrecy?
   ❌ Yes, if the PRD is really "secure": NO  
   ❌ No there are no ciphers with perfect secrecy: NO, because of one time pad
   ❌ Yes, every cipher has perfect secrecy: NO, παρά είναι αισιόδοξο
   ✓ No, cince the key is shorter than the message: YES.

CPA = Chosen Plaintext Attack
Μπορεί δηλαδή ο επιτιθέμενος να επιλέγει να διαλέγει plaintext.
