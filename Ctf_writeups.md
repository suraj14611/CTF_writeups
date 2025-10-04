# LINUX LUMINARIUM

## CHALLENGE NAME
Split-piping stderr and stdout

## CHALLENGE
/challenge/hack: this produces data on stdout and stderr
/challenge/the: you must redirect hack's stderr to this program
/challenge/planet: you must redirect hack's stdout to this program

### SOLVE
**Flag:** pwn.college{sk7kKiRkU0HHlEE5j95HOewricM.QXxQDM2wSM0cDOzEzW}
We can use > to redirect the stdout of hack to the command using >(/challenge/planet) and stderr of hack to the command >(/challenge/the) using 2>.


### COMMANDS
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack > >(/challenge/planet) 2> >(/challenge/the)
```



## CHALLENGE NAME
Multiple Globs

## CHALLENGE
 We put a few happy, but diversely-named files in /challenge/files. Go cd there and run /challenge/run, providing a single argument: a short (3 characters or less) globbed word with two * globs in it that covers every word that contains the letter p.

### Solve
**Flag:** pwn.college{kWCuaq31s60zkJoe3LdoVqWb28J.0lM3kjNxwSM0cDOzEzW}
Since '*' can be replaced with any character/characters including empty spaces we can use it to find all the words which contains letter 'p' in them.

### commands
hacker@globbing~multiple-globs:~$ cd /challenge/files
hacker@globbing~multiple-globs:/challenge/files$ /challenge/run *p*
```

# WEB EXPLOITATION

## CHALLENGE NAME
HTML - Source code

## CHALLENGE
To find the password which is hidden within the source code

### Solve
password : nZ^&@q5&sjJHev0
It is a simple challenge where the password can be viewed by just opening the page's source code.
```


## CHALLENGE NAME
HTTP - IP restriction bypass

## CHALLENGE
The challenge is to bypass the restriction that only devices with internal ip addresses can access the website.

### Solve
password : Ip_$po0Fing
We used an technique called X-Forwarded-For spoofing where we add "X-Forwarded-For" header to the request packet and adding an internal ip address like 192.168.x.x making out device look legit to the internal network
```


## CHALLENGE NAME
HTTP - Open redirect

## CHALLENGE
The challenge is to exploit an open redirect vulnerability, where the website redirects users to a URL specified by user input without proper validation.

### Solve
password : open_r3dir3ct
The web application takes a URL parameter and redirects the user to the value provided in this parameter. Since we redirected the page to google.com by giving the URL and checksum of google in the header request, we successfully exploited the vulnerability, hence revealing the flag.
```


## CHALLENGE NAME
HTTP - User-agent

## CHALLENGE
The challenge is to get admin access to the website to reveal the password.

### Solve
password : rr$Li9%L34qd1AAe27
Here in the request header we change the user agent from our current device's details to "admin" which made the server to give us the admin level access, hence revealing the password.
```


## CHALLENGE NAME
HTTP - Directory indexing

## CHALLENGE
The challenge is to exploit a misconfigured web server that exposes the contents of directories when no index file is present, allowing unauthorized access to files.

### Solve
password : LINUX
When directory indexing is enabled, visiting a directory path (such as /admin/ or its parent) without an index file displays a list of all files and subfolders.

To solve the challenge:

We explored the parent directory and found a file named admin.txt.

Opening admin.txt revealed the password: LINUX.

This password was then used to complete the challenge.
```


# CRPYTOGRAPHY

## CHALLENGE
Make a small python script for Hill Cipher, Encryption and Decryption and
an option to brute force it with known block size-(2x)

### SOLVE
# tiny_hill_2x2.py — minimal 2x2 Hill cipher
ALPH = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
M = 26

def egcd(a,b):
    if b==0: return (1,0,a)
    x,y,g = egcd(b, a%b)
    return (y, x - (a//b)*y, g)

def modinv(a):
    a %= M
    x,y,g = egcd(a, M)
    return x%M if g==1 else None

def inv2(k):
    a,b = k[0]; c,d = k[1]
    det = (a*d - b*c) % M
    invd = modinv(det)
    if invd is None: return None
    return [[( d*invd) % M, ((-b)*invd) % M],
            [((-c)*invd) % M, ( a*invd) % M]]

def norm(s): return ''.join(ch for ch in s.upper() if ch.isalpha())
def pad(s): return s + ('X' if len(s)%2 else '')
def pair_vals(p): return [ALPH.index(p[0]), ALPH.index(p[1])]
def vals_pair(v): return ALPH[v[0]%M] + ALPH[v[1]%M]
def mul(mat, vec): return [ (mat[0][0]*vec[0]+mat[0][1]*vec[1])%M,
                            (mat[1][0]*vec[0]+mat[1][1]*vec[1])%M ]

def encrypt(pt, key):
    s = pad(norm(pt))
    return ''.join(vals_pair(mul(key, pair_vals(s[i:i+2]))) for i in range(0,len(s),2))

def decrypt(ct, key):
    s = pad(norm(ct))
    invk = inv2(key)
    if invk is None: raise ValueError("Key not invertible")
    return ''.join(vals_pair(mul(invk, pair_vals(s[i:i+2]))) for i in range(0,len(s),2))

def brute_force(ct, crib):
    s = norm(ct)
    crib = crib.upper()
    found = []
    for a in range(26):
      for b in range(26):
        for c in range(26):
          for d in range(26):
            key = [[a,b],[c,d]]
            if inv2(key) is None: continue
            pt = decrypt(s, key)
            if crib in pt:
                found.append((key, pt))
    return found

if __name__ == "__main__":
    # quick demo
    K = [[3,3],[2,5]]              # example key
    print("Encrypt HELLO ->", encrypt("HELLO", K))
    print("Decrypt with same key ->", decrypt(encrypt("HELLO", K), K))
    # brute-force example (slow): uncomment to use
    # print("Bruteforce candidates:", brute_force("ciphertextHERE", "KNOWN"))
```


## CHALLENGE
cipher: book.txt - can you help my find the name of this book and it’s writer?
Link: https://drive.proton.me/urls/7HMYHJQB20#3ssNqYMRXlKp

### SOLVE
Author: James Joyce ; Book: Ulysses

Here the letters are replaced with emoji's, hence we can use frequency analysis to count the frequencies of each emoji and substitute them with the corresponding letters in English. Here is the following substitution table:

| Emoji | Letter |
| ----- | ------ |
| 😍    | E      |
| 🤡    | T      |
| 😙    | A      |
| 🥶    | O      |
| 🥴    | N      |
| 😢    | H      |
| 🥺    | S      |
| 😉    | R      |
| 🙀    | L      |
| 👂    | I      |
| 👧    | U      |
| 🥳    | D      |
| 😡    | M      |
| 🤩    | C      |
| 🙄    | Y      |
| 😎    | F      |
| 😃    | W      |
| 🤐    | G      |

```



## CHALLENGE
Decrypt cipher:taskphaWL_PL4sOingpYefdngaP{_diddL40ap}y5rn_s1m37 which was encrypted using spiral cypher

### SOLVE
taskphase{4r73m1s_n0_fOWL_PL4YPL5y}paddingpadding

To decrypt the given message first we have to arrange all the letters in square/rectangular box and read it clock wise starting from top left, here's how we can arrange it:

Row 1: T a s k p h a
Row 2: W L - P L 4 s
Row 3: O i n g p y e
Row 4: f d n g a p {
Row 5: - d i d d L 4
Row 6: 0 a p j y 5 r
Row 7: n - s 1 m 3 7
```






