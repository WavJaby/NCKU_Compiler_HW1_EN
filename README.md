# NCKU 1142 Compiler Construction — Homework 1

> For any questions about the test cases, feel free to contact the TA (Email: F74114760@gs.ncku.edu.tw, or Moodle message).

See also: [Semester Homework Overview](https://hackmd.io/@WavJaby/NCKU_1142_COMPILER_HW)

---

## Environment Setup

You may use any editor you are comfortable with.

### Windows

First, install [MSYS2](https://www.msys2.org/). After installation, open the **MSYS2 UCRT64** terminal and run the following commands:

```shell
pacman -Syu
pacman -S --needed mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-cmake mingw-w64-ucrt-x86_64-ninja flex
```

After installation, add the executables to your PATH:

- Open **Settings → System → About → Advanced system settings**
- Click **Environment Variables**, find `Path` under user variables, and add:

```
C:\msys64\usr\bin
C:\msys64\ucrt64
```

> The default MSYS2 installation path is `C:\msys64`. If you changed it during installation, replace the paths above accordingly.

After clicking OK to close all windows, open a new terminal and verify the installations:

```
gcc --version
cmake --version
ninja --version
flex -V
```

### Linux

- **Ubuntu / Debian / Linux Mint / WSL Ubuntu:**

```shell
sudo apt update
sudo apt install -y build-essential cmake flex git
```

- **Arch Linux:**

```shell
sudo pacman -Syu
sudo pacman -S base-devel cmake flex git
```

---

## Project Setup

Clone the assignment template and test cases:

```shell
git clone --recurse-submodules https://github.com/WavJaby/NCKU_Compiler_HW1_EN
```

### Running Tests

Running the tests will show your current score — what you see is what you get.

**Windows PowerShell:**

```shell
.\test.ps1 -English
```

> If you encounter an error like `cannot be loaded ... is not digitally signed`, run the following first:
> ```shell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

**Linux:**

```shell
chmod +x test.sh
./test.sh --english
```

### Debug Flags

- `-Interactive` (Linux: `--interactive`, `-i`): Navigate each diff with arrow keys for detailed debugging.
- `-StopOnFirstError` (Linux: `--stop-on-first-error`, `-s`): Stop at the first error instead of printing all files.

---

## Submission

We provide a server (Ubuntu 20.04) as a unified grading environment. Please use it responsibly — any attempt to disrupt or abuse the system will result in account suspension.

### Login

Connect to [NCKU VPN](https://ncku.twaren.net) before logging in:

```bash
ssh StudentID@140.116.154.65 -p 20261
```

Your username is your student ID. The default password is `5c7fca920f67869`. You will be forced to change it on first login.

> If you are locked out or forget your password, email p76144891@gs.ncku.edu.tw with the subject `[編譯系統] 伺服器帳號問題`.

### Upload Your Work

```bash
scp -P 20261 -r /PATH/TO/YOUR/LOCALFILE StudentID@140.116.154.65:/home/StudentID
```

Replace `/PATH/TO/YOUR/LOCALFILE` with your local file path and `StudentID` with your student ID.

After uploading, SSH back into the server to verify your files are there.

### Testing on the Server (HW1)

Grant execute permission to the test script:

```bash
chmod +x test.sh
```

Then run:

```bash
lexer_test -e
```

The default behavior stops at the first error. If you see line-ending issues (caused by editing on Windows), fix them with:

```bash
sed -i 's/\r$//' test.sh
```

> Note: Because many students share the server, your program runs inside a pre-configured container. You do not have permission to install additional software. Grading is based on the server results.

### Submitting (HW1)

Create a folder named `HW1` (uppercase) in your home directory and place the `NCKU_Compiler_HW1_EN` folder inside it:

```bash
cd ~
mkdir HW1
```

> Recommended: create the `HW1` folder first, then `git clone` the template directly inside it.

Grading is based on the server results. The Moodle submission is for backup only — if you want to back up, compress your work into a `.zip` file.

### Download from Server

```bash
scp -P 20261 -r StudentID@140.116.154.65:/home/StudentID/Project /PATH/YOUWANT
```

Replace `Project` with your folder name on the server and `/PATH/YOUWANT` with your desired local path.

---

## Wenyan Token Reference (English Version)

### Decorators

`.` and `,` are decorative punctuation with no semantic meaning and can be ignored.

### Types & Literals

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `long` | Long integer | `OBJECT_TYPE_I64` |
| `num` | Number (integer or float) | `OBJECT_TYPE_NUM` |
| `str` | String | `OBJECT_TYPE_STR` |
| `bool` | Boolean | `OBJECT_TYPE_BOOL` |
| `auto` | Dynamic / any type | `OBJECT_TYPE_AUTO` |
| `list` | Array | `OBJECT_TYPE_ARRAY` |
| `func` | Function type | `OBJECT_TYPE_FUNC` |
| `true` | Boolean true | `BOOL_LIT true` |
| `false` | Boolean false | `BOOL_LIT false` |

**Examples:**
- **Number (`num`):** `i have 1 num. called 1. name it [己].`
- **String (`str`):** `i have 1 str. called [[噫吁戲！]]. name it [唉呀].`
- **Boolean (`bool`):** `i have 1 bool. called false. name it [陰陽].`
- **Auto type (`auto`):** `i have 1 auto. called 1. name it [隨義].`
- **Array (`list`):** `i have 1 list. name it [五音].`

Strings are wrapped in double brackets **`[[ ... ]]`**.  
Identifiers (variable names) are wrapped in single brackets **`[ ... ]`**.

> **Challenge:** Parse `[[ a string containing ] inside ]]` and `[[ last char is ] ]]` correctly — when reading a string, if only one `]` is encountered, keep reading until two consecutive `]]` are found.

---

### Variables & Assignments

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `has a` | Declare a single variable | `HERE_IS_A` |
| `i have`, `now have` | Declare multiple variables | `HERE_ARE` |
| `name it` | Name it | `NAME_IT` |
| `called` | Value is / assign value | `SAID` |
| `prev` | Previous variable | `PAST` |
| `topic` | Topic marker | `TOPIC` |
| `set` | Set / now | `SET` |
| `its` | Last result | `ITS` |
| `is thus` | Assignment end | `IS_THUS` |

**Examples:**
- **Declare multiple variables:** `i have 2 num. called 3. called 5. name it [乙] called [丙].`
- **Assign from variable:** `i have 1 num. called [己]. name it [丁].`
- **Update a variable (`prev...set its is thus`):** `prev [丙] topic. set [大衍] is thus.`
- **Self-increment:** `add 3 <- [去路]. prev [去路] topic. set its is thus.`

---

### Math & Logic Operators

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `add` | Addition | `OP_ADD` |
| `sub` | Subtraction | `OP_SUB` |
| `mul` | Multiplication | `OP_MUL` |
| `div` | Division | `OP_DIV` |
| `mod` | Modulo | `OP_MOD` |
| `<-` | Preposition left operand | `<-` |
| `->` | Preposition right operand | `->` |
| `partially true` | Logical OR | `OP_OR` |
| `are true` | Logical AND | `OP_AND` |
| `>` | Greater than | `OP_GT` |
| `<` | Less than | `OP_LT` |
| `<=` | Less than or equal | `OP_LE` |
| `>=` | Greater than or equal | `OP_GE` |
| `==` | Equal | `OP_EQ` |
| `!=` | Not equal | `OP_NE` |

**Examples:**
- **Add / Subtract:** `add [甲] <- [乙]. name it [和].` / `sub [甲] -> [乙]. name it [差].`
- **Multiply / Divide:** `mul [甲] -> [乙]. name it [積].` / `div [甲] -> [乙]. name it [商].`
- **Modulo:** `div [甲] -> [乙]. mod. name it [餘].`
- **Comparison:** `if [尋訪線索] > 2 topic.`
- **Logical OR:** `if those [雲] [霧] partially true topic.`
- **Logical AND:** `if those [雲] [霧] are true topic.`

---

### Control Flow

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `if` | If | `IF` |
| `else if` | Else if | `ELSE_IF` |
| `else` | Else | `ELSE` |
| `for` | For loop | `FOR` |
| `times` | Loop count | `TIMES` |
| `while true` | Infinite loop | `WHILE_TRUE` |
| `break` | Break | `BREAK` |
| `return` | Return | `RETURN` |
| `end` | End of block | `END` |

**Examples:**

- **Conditional (`if...else if...else`):**

```wenyan
if [尋訪線索] == 1 topic.
    has a str [[童子答曰：『言師採藥去。』]] print.
else if [尋訪線索] == 2 topic.
    has a str [[童子指山曰：『只在此山中。』]] print.
else.
    has a str [[奇了，童子竟言他物！]] print.
end.
```

- **Count loop (`for...times`):**

```wenyan
for 2 times.
    i have 1 str. called [[一願郎君千歲，二願妾身常健。]]. print.
end.
```

---

### Functions

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `to perform` | Define a function | `TO_PERFORM_FUNC` |
| `requires` | Require arguments | `REQUIRE_ARGS` |
| `func begin` | Function body start | `FUNC_BEGIN` |
| `func end for` | Function end marker | `FUNC_END_FOR` |
| `func end` | Function end | `FUNC_END` |
| `call` | Call a function | `CALL` |

**Example — define and call a function:**

```wenyan
i have 1 func. name it [牛頓求根法]. to perform. requires 1 num. called [試]. func begin.
    has a num 1. name it [甲].
    ...
    return [甲].
func end for [牛頓求根法] func end.

call [牛頓求根法] <- 2. print.
```

---

### Arrays & Other Keywords

| English Token | Meaning | Token Name |
| :--- | :--- | :--- |
| `of` | Array index access | `INDEX` |
| `push` | Push element | `PUSH` |
| `length` | Array / string length | `ARRAY_LENGTH` |
| `print` | Print / output | `PRINT` |
| `those` | Extract expression | `THOSE` |
| `take` | Take | `TAKE` |
| `NOTE` | Comment | `COMMENT` |

**Examples:**
- **Push elements:** `push [五音] -> [[宮]]. -> [[商]]. -> [[角]].`
- **Array length:** `those [五音] length. print.`
- **Array index:** `those [五音] of [甲]. print.` / `those [正讀] of [索引]. name it [單字].`
- **Print:** `i have 2 str. called [[倒向讀之：]]. called [倒讀]. print.`

---

## Homework 1: Lexical Analysis (Scanner)

### Objective

Implement a Scanner for the English version of the Wenyan programming language using **Flex**.

### What You Need to Do

Modify the `compiler.l` template in the `/src` directory so that it correctly identifies and outputs every token with its position information.

> All test cases are located in the `test/` folder — there are no hidden test cases. It is recommended to work through them one by one. All token definitions are in the tables above — once you have correctly defined all tokens, the homework is complete.

### Example Input (`test/questions_en/01_declare_naming.wy`)

```
NOTE  [[
    斯篇專作變數立名與賦值之驗證。
    考其立數、紀言、定陰陽之法，
    以驗編譯器轉譯之效。
]]
i have 1 bool. called false. name it [陰陽].               NOTE [[ bool 陰陽 = false; ]]
i have 1 num. called 1. name it [甲].                 NOTE [[ int 甲 = 1; ]]
i have 1 num. called 1.2. name it [乙].              NOTE [[ double 乙 = 1.2; ]]
has a bool. name it [陰].                           NOTE [[ bool 陰; // Default false ]]
has a num. name it [零].                           NOTE [[ int 零; // Default 0 ]]
```

### Example Output (`test/answers_en/01_declare_naming.out`)

```
1:1: COMMENT NOTE
1:7: STR_LIT "
    斯篇專作變數立名與賦值之驗證。
    考其立數、紀言、定陰陽之法，
    以驗編譯器轉譯之效。
"
7:1: HERE_ARE i have
7:8: NUMBER_LIT 1
7:10: VAR_TYPE bool
7:16: SAID called
7:23: BOOL_LIT false
7:30: NAME_IT name it
7:38: IDENT '陰陽'
7:58: COMMENT NOTE
7:63: STR_LIT " bool 陰陽 = false; "
8:1: HERE_ARE i have
8:8: NUMBER_LIT 1
8:10: VAR_TYPE num
8:15: SAID called
8:22: NUMBER_LIT 1
8:25: NAME_IT name it
8:33: IDENT '甲'
8:54: COMMENT NOTE
8:59: STR_LIT " int 甲 = 1; "
9:1: HERE_ARE i have
9:8: NUMBER_LIT 1
9:10: VAR_TYPE num
9:15: SAID called
9:22: NUMBER_LIT 1.2
9:27: NAME_IT name it
9:35: IDENT '乙'
9:53: COMMENT NOTE
9:58: STR_LIT " double 乙 = 1.2; "
11:1: HERE_IS_A has a
11:7: VAR_TYPE bool
11:13: NAME_IT name it
11:21: IDENT '陰'
11:52: COMMENT NOTE
11:57: STR_LIT " bool 陰; // Default false "
12:1: HERE_IS_A has a
12:7: VAR_TYPE num
12:12: NAME_IT name it
12:20: IDENT '零'
12:51: COMMENT NOTE
12:56: STR_LIT " int 零; // Default 0 "
```

The output format for each token is:

```
<line>:<column>: <TOKEN_NAME> <matched_text>
```

---

## How Flex Works

Flex reads a `.l` file with token rules and generates a C scanner (`lex.yy.c`).

**Workflow:**
1. Write a `.l` file defining token patterns
2. Run `flex file_name.l` to generate `lex.yy.c`
3. Compile the C code
4. Run the scanner executable

### Minimal Example (`cpp_scanner.l`)

```cpp
%{
#include <stdio.h>
%}
%option noyywrap
%%
/* Left side: token pattern (regex); Right side: C code to execute on match */
"int"|"float"|"char"|"if"|"else"|"while"|"return"  { printf("[KEYWORD]: %s\n", yytext); }
[a-zA-Z_][a-zA-Z0-9_]*                             { printf("[ID]: %s\n", yytext); }
[0-9]+                                              { printf("[NUMBER]: %s\n", yytext); }
"<<"|">>"|"=="|"!="                                 { printf("[OPERATOR]: %s\n", yytext); }
"="|"+"|"-"|"*"|"/"                                 { printf("[OPERATOR]: %s\n", yytext); }
";"|","|"("|")"|"{"|"}"                             { printf("[PUNCT]: %s\n", yytext); }
[ \t\n]+                                            { /* ignore whitespace */ }
.                                                   { printf("[ERROR]: Unknown character '%s'\n", yytext); }
%%
int main() {
    printf("--- Scanner Start ---\n");
    yylex();
    printf("--- Scanner End ---\n");
    return 0;
}
```

> `yytext` holds the raw matched string.

Generate and run the scanner:

```shell
flex cpp_scanner.l
gcc lex.yy.c -o cpp_scanner
./cpp_scanner
```

Given input:

```cpp
int count = 10;
if (count == 10) return 0;
```

Expected output:

```
[KEYWORD]: int
[ID]: count
[OPERATOR]: =
[NUMBER]: 10
[PUNCT]: ;
[KEYWORD]: if
[PUNCT]: (
[ID]: count
[OPERATOR]: ==
[NUMBER]: 10
[PUNCT]: )
[KEYWORD]: return
[NUMBER]: 0
[PUNCT]: ;
```

From this example we can see that Flex's job is simple: define the token rules, and it generates the scanner C code for you. Your task in this homework is to do exactly that — for the Wenyan language.

### Flex Matching Rules

#### Longest match wins

The rule that matches the most characters is chosen. For example:

```cpp
"="  { printf("ASSIGN\n"); }
"==" { printf("EQUAL\n"); }
```

When the input is `==`, both rules match, but `==` matches two characters while `=` matches only one — so the second rule wins.

#### First rule wins on tie

If two rules match the same length, the one defined earlier takes priority. For example:

```cpp
"int"              { printf("KEYWORD_INT\n"); }
[a-z][a-z0-9]*    { printf("IDENTIFIER\n"); }
```

When the input is `int`, both rules match 3 characters, but `"int"` is defined first — so it wins.

### Flex State Switching

Flex supports exclusive states (`%x STATE_NAME`) for handling complex tokens like multi-line strings or comments. Use `BEGIN(STATE)` to enter a state and `BEGIN(INITIAL)` to return to the default state.

Example:

```cpp
%{
#include <stdio.h>
%}
%x COMMENT
%%
"/*"  { printf("Enter comment\n"); BEGIN(COMMENT); }
<COMMENT>"*/"  { printf("Exit comment\n"); BEGIN(INITIAL); }
<COMMENT>.     { /* comment body, do nothing */ }
<COMMENT>\n    { /* ignore newline */ }
"int"          { printf("keyword: int\n"); }
[0-9]+         { printf("number: %s\n", yytext); }
[ \t\n]+       ;  /* ignore whitespace */
.              { printf("other: %s\n", yytext); }
%%
int main() { yylex(); return 0; }
```

Once `/*` is matched, the scanner enters the `COMMENT` state. Only rules tagged `<COMMENT>` apply until `BEGIN(INITIAL)` is called. Rules outside `<COMMENT>` have no effect while inside the state, and vice versa.

> You can define multiple named states. Rules in different states do not interfere with each other.

For more details, see the [Flex manual](https://westes.github.io/flex/manual/) and [Flex GitHub repository](https://github.com/westes/flex).

---

## UTF-8 Handling in Flex

Flex does not natively support UTF-8. It operates byte-by-byte. However, for most tokens you can simply wrap them in quotes (e.g., `"i have"`) and Flex will match the exact byte sequence — no special handling needed.

[UTF-8 Range Definition](https://www.w3.org/2005/03/23-lex-U#validutf8)

The tricky part is when you need to match **any** UTF-8 character (e.g., inside a string or identifier). The template provides pre-defined UTF-8 regex patterns:

```c
ASCII    [\x00-\x7f]
U        [\x80-\xbf]
B2       [\xC2-\xDF]{U}
B3_1     \xE0[\xA0-\xBF]{U}
B3_2     [\xE1-\xEC]{U}{U}
B3_3     \xED[\x80-\x9F]{U}
B3_4     [\xEE-\xEF]{U}{U}
B4_1     \xF0[\x90-\xBF]{U}{U}
B4_2     [\xF1-\xF3]{U}{U}{U}
B4_3     \xF4[\x80-\x8F]{U}{U}

ALL_UTF8 {ASCII}|{B2}|{B3_1}|{B3_2}|{B3_3}|{B3_4}|{B4_1}|{B4_2}|{B4_3}
```

Each pattern corresponds to a specific Unicode range:

| Pattern | Byte 1 | Byte 2 | Byte 3 | Byte 4 | Unicode Range |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **ASCII** | `00`–`7F` | — | — | — | `U+0000`–`U+007F` (standard ASCII) |
| **B2** | `C2`–`DF` | `80`–`BF` | — | — | `U+0080`–`U+07FF` (excludes overlong `C0`–`C1`) |
| **B3_1** | `E0` | **`A0`–`BF`** | `U` | — | `U+0800`–`U+0FFF` (excludes overlong 2-byte range) |
| **B3_2** | `E1`–`EC` | `80`–`BF` | `U` | — | `U+1000`–`U+CFFF` |
| **B3_3** | `ED` | **`80`–`9F`** | `U` | — | `U+D000`–`U+D7FF` (excludes surrogate pairs `D800`–`DFFF`) |
| **B3_4** | `EE`–`EF` | `80`–`BF` | `U` | — | `U+E000`–`U+FFFF` |
| **B4_1** | `F0` | **`90`–`BF`** | `U` | `U` | `U+10000`–`U+3FFFF` (excludes overlong 3-byte range) |
| **B4_2** | `F1`–`F3` | `80`–`BF` | `U` | `U` | `U+40000`–`U+FFFFF` |
| **B4_3** | `F4` | **`80`–`8F`** | `U` | `U` | `U+100000`–`U+10FFFF` (capped at Unicode maximum) |

### Excluding `」` from String Matching

When reading a Wenyan string delimited by `「「...」」`, you cannot use `{ALL_UTF8}*` directly — Flex's longest-match rule would cause the `{ALL_UTF8}*` pattern to greedily consume the closing `」」`, preventing it from ever being matched.

The only option is to construct a regex that covers every UTF-8 character **except** `」`. You might wonder: why not just use `[^...]` to exclude it? For plain ASCII that works perfectly, but `」` is a 3-byte UTF-8 character (`\xE3\x80\x8D`). Flex would interpret `[^」]` as "exclude any of the three individual bytes `\xE3`, `\x80`, or `\x8D`" — completely wrong.

So the template provides `B3_2_EXCLUDE_QUO` and `EXCLUDE_QUO` to surgically remove `」` from the full UTF-8 range:

```c
B3_2_EXCLUDE_QUO ([\xE1-\xE2]{U}{U})|(\xE3\x80[\x80-\x8C\x8E-\xBF])|(\xE3[\x81-\xBF]{U})|([\xE4-\xEC]{U}{U})

EXCLUDE_QUO [\x20-\x7E]|{B2}|{B3_1}|{B3_2_EXCLUDE_QUO}|{B3_3}|{B3_4}|{B4_1}|{B4_2}|{B4_3}
```

#### How `B3_2_EXCLUDE_QUO` works

First, look up `」` with a [Unicode code converter](https://r12a.github.io/app-conversion/): its UTF-8 encoding is `E3 80 8D`. Cross-referencing the table above, it falls squarely inside the `B3_2` range (`E1`–`EC` leading byte).

The goal is to write a regex that covers all of `B3_2` except the single code point `\xE3\x80\x8D`. `B3_2_EXCLUDE_QUO` is split into four parts separated by `|`:

1. **`([\xE1-\xE2]{U}{U})`** — leading byte `E1` or `E2`. Our target starts with `E3`, so this entire sub-range is safe to pass through entirely.
2. **`(\xE3\x80[\x80-\x8C\x8E-\xBF])`** — the surgical cut. When leading byte is `E3` and second byte is `80`, we are in the danger zone. Instead of using the catch-all `{U}` for the third byte, we split it into `80`–`8C` and `8E`–`BF`, precisely skipping `8D`.
3. **`(\xE3[\x81-\xBF]{U})`** — leading byte `E3`, second byte `81` or above. This is past the danger zone; all such sequences are safe.
4. **`([\xE4-\xEC]{U}{U})`** — leading byte `E4` through `EC`. Completely clear of `E3`; all pass through.

The result: every UTF-8 character from `B3_2` is accepted, except `」` (`\xE3\x80\x8D`).

#### Why `[\x20-\x7E]` instead of `[\x00-\x7F]`?

This is a deliberate safety technique. By starting from `\x20` instead of `\x00`, we exclude all ASCII control characters (`\x00`–`\x1F`), most importantly the newline `\n`.

In Wenyan, strings cannot span multiple lines. If a programmer forgets the closing `」」`, the scanner will hit the `\r?\n` rule at the end of the line and can report an error — instead of silently consuming the rest of the source file as string content. (This error-recovery technique will be explored further in a future assignment.)

### Why `{U}` is `[\x80-\xbf]`

UTF-8's **self-synchronization** design ensures that continuation bytes always start with `10` in binary. The minimum value `10000000` = `0x80` and the maximum `10111111` = `0xBF`. This means a parser can always tell whether a byte is a leading byte or a continuation byte, making the encoding robust against partial reads.

This design is considered brilliant because in early networking, dropped packets could corrupt multi-byte sequences. With UTF-8:

- `0xxxxxxx` (`00`–`7F`): immediately recognized as ASCII
- `11xxxxxx` (`C2`–`F4`): start of a multi-byte character
- `10xxxxxx` (`80`–`BF`): continuation byte — cannot be a new character start

If a parser starts reading from the middle of a string, it can detect continuation bytes and skip forward until it finds a valid leading byte. This completely avoids the catastrophic cascading corruption that plagued older encodings like Big5, where losing a single byte could corrupt thousands of subsequent characters.

### Unicode vs UTF-8: What's the Difference?

These two terms are often confused, but they are different things:

**Unicode** is a universal character registry — it assigns a unique code point to every character, symbol, and emoji in every language. For example, `」` is `U+300D` and 😊 is `U+1F60A`. Unicode only defines *what* a character is; it says nothing about how to store it.

**UTF-8** is an *encoding* — a method for converting Unicode code points into bytes that can be stored or transmitted. It uses a variable-length design: ASCII characters use 1 byte, most CJK characters use 3 bytes, and emoji use 4 bytes. This makes it both compact (for ASCII-heavy text) and universal (for all Unicode characters), which is why UTF-8 has become the default encoding for the web and most modern programming languages.

In short: **Unicode defines "what character this is"; UTF-8 defines "how to store it in bytes."**

