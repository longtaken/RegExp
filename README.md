# RegExp
javascript正则表达式
##正则表达式中的直接量字符
###直接量字符
| 字符 | 匹配 |
| :-------------- | :------------ |
| \o | NUL字符(\u0000) |
| \t | 制表符(\u0009)（tab） |
| \n | 换行符(\u000A) |
| \v | 垂直制表符(\u000B) |
| \f | 换页符(\u000C) |
| \r | 回车符(\u000D) |
| \xnn | 由十六进制数nn指定的拉丁字符，例如\xOA等价于\n |
| \uxxx | 由十六进制数xxxx指定的Unicode字符，例如\u009等价于\t |
| \cX | 控制字符^X，例如\cJ等价于换行符\n |
* \s匹配单个空格，等同于[\f\n\r\t\v]
    
###字符类
| 字符 | 匹配 |
| :-------------- | :------------ |
| [...] | 方括号内的任意字符 |
| [^...] | 不在方括号内的任意字符 |
| . | 除换行符和其他unicode行终止符之外的任意字符 |
| \w | 任何ASCII字符组成的单词，等价于[a-zA-Z0-9_] |
| \W | 任何不适ASCII字符组成的单词，等价于[^a-zA-Z0-9_] |
| \s | 任何unicode空白符 |
| \S | 任何非unicode空白符的字符，注意\w跟\S不同 |
| \d | 任何ASCII数字，等价于[0-9] |
| \D | 除了ASCII数字之外的任何字符，等价于[^0-9] |
| [\b] | 退格直接量（特例） |

###重复
| 字符 | 含义 |
| :-------------- | :------------ |
| {n,m} | 匹配前一项至少n次，不超过m次 |
| {n,} | 匹配前一项n次或者更多次 |
| {n} | 匹配前一项n次 |
| ? | 匹配前一项0次或者1次，即前一项为可选  /a+?b/ 指的是+号可有可无 |
| + | 匹配前一项1次或者多次，等价于{1,} |
| * | 匹配前一项0次或者多次，等价于{0,} |

###选择、分组和引用
| 字符 | 含义 |
| :-------------- | :------------ |
| | | 选择，匹配的是该符号左边的子表达式或者右边的子表达式 |
| (...) | 组合，将几个项组合为一个单元，这个单元可通过* + ?等符号加以修饰。 |
| (?:...) | 只组合，把项组合到一个单元，但不记忆与该组相匹配的字符 |
| \n | 和第n个分组第一次匹配的字符相匹配，组是圆括号中的子表达式(也有可能是嵌套的) |		
				
###锚字符
| 字符 | 含义 |
| :-------------- | :------------ |
| ^ | 匹配字符串的开头，在多行检索中，匹配一行的开头 |
| $ | 匹配字符串的结尾，在多行检索中，匹配一行的结尾 |
| \b | 匹配一个单词的边界，位于字符\w和\W之间的位置，或位于字符\w和字符串的开头或者结尾之间的位置 |
| \B | 匹配非单词边界的位置 |
| (?=p) | 零宽正向先行断言，要求接下来的字符都与p匹配，但不能包括匹配p的那些字符 |
| (?!p) | 零宽负向先行断言，要求接下来的字符不与p匹配 |
				
###修饰符
| 字符 | 含义 |
| :-------------- | :------------ |
| i | 不区分大小写的匹配 |
| g | 执行一个全局匹配，不是找到第一个之后停止 |
| m | 多行匹配模式。^匹配一行的开头和字符串的开头，$匹配行的结束和字符串的结束 |

##非贪婪的重复
匹配重复字符尽可能地多匹配，而且允许后续的正则表达式继续匹配，称之为“贪婪的”匹配。
我们在待匹配的字符后跟随一个问号即表示非贪婪匹配。
* 使用贪婪匹配可能得到的结果跟预期不一致。
如：针对"aaab"，贪婪匹配/a+b/的结果跟非贪婪/a+?b/的结果是一样的。因为正则表达式的模式匹配总是会寻找字符串中第一个可能匹配的位置，由于该匹配时从字符串的第一个字符开始的，因此在这里不考虑它的子串中更短的匹配。


##匹配原理
###占有字符和零宽度
* 占有字符
在正则表达式匹配的过程中，如果子表达式匹配到的是字符内容，而非位置的话，并被保存在匹配的结果当中，那么就认为该子表达式是占有字符的
* 零宽度
如果子表达式匹配的仅仅是位置，或者说匹配中的内容不保存到匹配的结果当中，那么就认为该子表达式是零宽度的

###环视
环视只进行子表达式匹配，不占有字符，匹配到的内容不保存到最终的匹配的结果，是零宽度的，它匹配的结果就是一个位置；
环视的作用相当于对所在的位置加了一个附加条件，只有满足了这个条件，环视子表达式才能匹配成功。
javascript中只支持顺序环视
* (?=Expression):  顺序肯定环视，含义是所在的位置右侧位置能够匹配到regexp.
* (?!Expression):  顺序否定环视，含义是所在的位置右侧位置不能匹配到regexp.

###反向引用
* 捕获性分组取到的内容，不仅可以在正则表达式外部通过程序进行引用，也可以在正则表达式内部进行引用，这种引用方式就叫做反向引用。
* 反向引用的作用是：是用来查找或限定重复，查找或限定指定标识配对出现等。


##String中4种支持使用正则表达式的方法

###search---stringObject.search(regexp);
* 它的参数是一个正则表达式，返回第一个与之匹配的子串的起始位置，如果找不到匹配的子串，返回-1
* search的参数不是正则表达式则首先会通过RegExp构造函数将它转换成正则表达式
* search方法不支持全局检索，忽略修饰符g
* 没有regexp对象的lastIndex的属性，且总是从字符串开始位置进行查找，总是返回的是stringObject匹配的第一个位置

###match---stringObject.match(regexp);
* 唯一参数是正则表达式（或者通过RegExp转换）,返回的是一个由匹配结果组成的数组
* 如果该正则表达式设置了修饰符g，则该方法返回的数组包含字符串中所有匹配的结果
* 如果没有匹配，那么它将返回的是null
* 返回的数组内有三个元素，第一个元素的存放的是匹配的文本。还有二个对象属性：
	* index属性表明的是匹配文本的起始字符在stringObject中的位置
	* input属性声明的是对stringObject对象的引用
	
###replace---stringObject.replace(regexp/substr,replacement);
* 第一个参数是正则表达式，第二个参数是要进行替换的字符串或者一个函数
* 如果第一个参数是字符串而不是正则表达式，replace直接搜索这个字符串
* 特殊字符
	* $1-$99	表示与regexp中的第1到第99个子表达式相匹配的文本
	* RegExp['$&']	返回正则表达式匹配项的字符串
	* RegExp["$'"]	返回被搜索的字符串中从最后一个匹配位置开始到字符串结尾之间的字符
	* RegExp['$`']	返回被查找的字符串从字符串开始的位置到最后匹配之前的位置之间的字符
	* RegExp['$+']  返回任何正则表达式查找过程中最后括号的子匹配
	* RegExp['$_']  返回任何正则表达式搜索过程中的最后匹配的字符
* 如果第二个参数是函数，函数的返回值是结果想替换的值。
	* 函数的第一个参数是匹配的字符串
	* 函数的第二个参数是正则表达式分组内容，没有分组的话，就没有该参数（如果没有该参数的话那么第四个参数就是undefined）
	* 函数的第三个参数是匹配项在字符串中的索引index
	* 函数的第四个参数是原字符串

###split---stringObject.split(separator,howmany);
* 参数一：字符串或正则表达式，该参数指定的地方分割
* 参数二：该参数指定返回的数组的最大长度，如果设置了该参数，返回的子字符串不会多于这个参数指定的数组。如果没有设置该参数的话，整个字符串都会被分割，不考虑他的长度

##RegExp中2个方法

###test---RegExpObject.test(str);
* 参数是需要检测的字符串
* 如果字符串中含有与正则匹配的文本的话，返回true，否则返回false

###exec---RegExpObject.exec(string);
* exec方法对一个指定的字符串执行一个正则表达式，没有找到匹配返回null，找到则返回数组
* exec带有g时，它把当前正则表达式对象的lastIndex属性设置为紧靠匹配子串的字符位置。当同一个正则表达式第二次调用exec的时候，它将从lastIndex属性所指示的字符处开始检索。没有匹配结果设置lastIndex为0
* 返回的数组的第一个元素是与正则表达式相匹配的文本，该方法还返回2个属性，index属性声明的是匹配文本的第一个字符的位置；input属性则存放的是被检索的字符串string；该方法如果不是全局的话，返回的数组与match()方法返回的数组是相同的
