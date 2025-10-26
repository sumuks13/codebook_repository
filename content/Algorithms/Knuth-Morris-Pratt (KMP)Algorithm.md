Tags : [[Algorithms]]

KMP algorithm is used for the string matching problem.
Time Complexity : O(m+n)

The basic idea behind KMP’s algorithm is: whenever we detect a mismatch (after some matches), we already know some of the characters in the text of the next window. We take advantage of this information to avoid matching the characters that we know will anyway match.

*Need of Preprocessing?*

An important question arises from the above explanation, how to know how many characters to be skipped. To know this,   
we pre-process pattern and prepare an integer array lps[] that tells us the count of characters to be skipped


**Example:** Given a string 'T' and pattern 'P' as follows:

![The Knuth-Morris-Pratt Algorithm](https://static.javatpoint.com/tutorial/daa/images/knuth-morris-pratt-algorithm2.png)

![[Pasted image 20240731063330.png]]
![[Pasted image 20240731063309.png]]
![[Pasted image 20240731063241.png]]
![[Pasted image 20240731063503.png]]
![[Pasted image 20240731063530.png]]


TODO
- Explain how basic substring search is done O(m x n)