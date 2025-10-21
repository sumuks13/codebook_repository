tags: [[Algorithms]]

https://stackoverflow.com/questions/62082297/how-to-determine-the-kth-character-in-the-concatenated-string

Given a list that contains **N** strings of lowercase English alphabets. Any number of contiguous strings can be found together to form a new string. The grouping function accepts two integers **X** and **Y** and concatenates all strings between indices **X** and **Y** (inclusive) and returns a modified string in which the alphabets of the concatenated string are sorted.

You are asked **Q** questions each containing two integers **L** and **R**. Determine the $K^{th}$. character in the concatenated string if we pass **L** and **R** to the grouping function.

### Input Format

- First Line: **N**(number of strings in the list)
- Next **N** lines: String $S_i$
- Next line **Q**(number of questions)
- Next **Q** lines : Three space-separated integers **L**, **R** and **K**

### Output Format

- For each question, print the $K^{th}$ character of the concatenated string in a new line.

--------------


> [!NOTE] Logic?
> ```code 
> list = ['aaaa', 'bbbb', 'cccc']
> for(int i=l;  i<=r; i++)
> 	str = str + list[i]
> 	if(str.length >= k)
> 		return str.charAt(k)

