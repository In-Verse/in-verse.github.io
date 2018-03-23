---
layout: post
title: "Files Full Of Whitespace"
date: 2018-03-22
---
<br>
<br>
<p>
I was working in Sysops when I encountered a frustrating problem. I was making a Windows Virtual Machine as a "test box" on my Archlinux work computer. I downloaded the Windows ISO from our network share. However, try as I might, I couldn't get VMbox to recognize the ISO as a <em>valid</em> option. 
<br>
<br>
I was totally confused for a good 10 minutes. Then, I brought me boss Peter in to be confused for another good 10 minutes. Some more time passed before we'd realized the simple, stupid mistake. It was because the Windows ISO ended in *capital* ISO extension instead of a <em>lowercase</em> iso extension. For Windows, the case doesn't matter. But, for Linux, the case *does* matter. Bad practices from file naming in Windows can come back to haunt you when using the files from a Linux OS. 
<br>
<br>
So, this got me to thinking about <b>bad filenames</b>. What would be the most annoying thing for a programmer to encounter? I first thought it might be something with abstract Unicode characters. But, then, I rememembered whitespace. <em>What if somebody encountered a folder full of "nothing"?</em> 
<br>
<br>
I remembered a silly space character that once caused some trouble in .NET. The Mongolian Vowel Separator has a value of U+180e. You probably don't use it in your regular speech (unless you are from Mongolia). However, the vowel separator has oscillated between being in the "formatting" category and "space" category. Because of this, you can create code that will actually <em>vary</em> based on the Unicode expression. You could insert a Mongolian Vowel Separator in a variable and the compiler will think nothing of it all - and then in other instances, your compiler will be bugged out. 
<br>
<br>
However, as of current, the Mongolian Vowel Separator is not classified as whitespace. So, what is whitespace? There are currently 25 characters defined as whitespace ("WS") in the Unicode Database. I choose from that to create a program that will create the max number of whitespace possible. 
<br>
<br>
Here is a basic implementation with the classic space. Note that you can replace it with the Unicode equivalent as long as you escape it with a `\`.
</p>
<br>
<br>
<pre class="prettyprint"><code class="language-no-highlight">
function makespace(){
  for i in {1..255}
    do
      space=$space' '
      touch "$1/$space"
    done
    space=''
}</code></pre>
<br>
<br>
It's that simple. 
<br>
<br>
Note that the loop goes to 255 because the max filelength of a file in a directory is 255 characters.
<br>
<br><br>
<h4>More Fun With SpaceMaker</h4>
<br>
<p>
The program can write spaces to one directory. But, you can extend it to write to all possible directories if you look for `find`able directories. Put the result in a list that the program can while loop over. Now, you can write to directories on the server that you have permission to.
</p>
<br>
<br>
<pre class="prettyprint"><code class="language-no-highlight">
find /home/ -type d \( -perm -o+w \) 2>/dev/null > ~/list.txt
</code></pre>
<br>
<br>
<p>
Create a distinct timestamp for each file. Note that you need to check month, day, hour, and minute values if its less than 10 because you need to append a 0; it's just how the command `touch` takes timestamps. 
</p>
<br>
<pre class="prettyprint"><code class="language-no-highlight">
function timestamp(){
  stamp=''
  local year=`shuf -i 1990-2018 -n 1`
  stamp="$stamp$year"
  local month=`shuf -i 1-12 -n 1`
  isone $month
  ...
}
</code></pre>
<br>
<br>
<p>
Make variable size of each file made of space with `fortune`. Why? Because directing the output of fortune in a file will mean that it will be harder for users to delete files based on a fixed-or-empty size value. 
<br>
<br>
The biggest `fortune` size would be from a Iain M. Banks quote that outputs a file of size 2435. And the smallest would be "Chess tonight.", which outputs a size of 15. This is a huge range.
<br>
<br>
Make a cronjob so that your program will relentlessly create spaces on a schedule. 
</p>
<pre class="prettyprint"><code class="language-no-highlight">
0 1 * * * /home/username/SpaceMaker.sh
</code></pre>

<h4>Why would you ever need files full of just whitespace?</h4>
<p>
I'll be honest, creating directories of whitespace is not practical. However, it's great to imagine. Having files full of whitespace means that you're going to start to be creative. If *all* the files are full of whitespace and you're looking for a secret/flag, what are you going to do? When you do a basic `ls`, the output will be, well, nothing. You can't rely on filenames. And, you don't want to `less` or `cat` through 255 files. This might force you to use regular expressions and `grep`.
</p>

<p>References:</p>
<ul>
  <li><a href="https://codeblog.jonskeet.uk/2014/12/01/when-is-an-identifier-not-an-identifier-attack-of-the-mongolian-vowel-separator/">Attack of the Mongolian File Separator</a></li>
    <li><a href="https://www.unicode.org/versions/Unicode10.0.0/">Unicode Standard</a></li>
</ul>
