# pythoncode
Python - unpack arguments from list with star operator
 
We can use *-operator (star operator) to unpack the arguments out of a list or tuple. Here is an example with python built in range()function :
>>> mylist = [3, 10]
>>> mylist
[3, 10]
>>> range(mylist[0], mylist[1])
[3, 4, 5, 6, 7, 8, 9]
>>> range(*mylist)
[3, 4, 5, 6, 7, 8, 9]
>>> 
 
 
Python sort file based on last field
Input file:

$ cat file.txt
IN,90,453
US,12,1,120
NZ,89,200
WI,500
TS,12,124

Required output: Sort the above comma delimited file based on the last field (column). i.e. required output:

US,12,1,120
TS,12,124
NZ,89,200
IN,90,453
WI,500

Solution:
The solution using Awk in UNIX bash shell is here. And here is the python one:

$ python
Python 2.5.2 (r252:60911, Jan 20 2010, 21:48:48)
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu3)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> d_list = [line.strip() for line in open("file.txt")]
>>> d_list
['IN,90,453', 'US,12,1,120', 'NZ,89,200', 'WI,500', 'TS,12,124']
>>> d_list.sort(key = lambda line: line.split(",")[-1])
>>> d_list
['US,12,1,120', 'TS,12,124', 'NZ,89,200', 'IN,90,453', 'WI,500']
>>> for line in d_list:
...     print line
...
US,12,1,120
TS,12,124
NZ,89,200
IN,90,453
WI,500
>>>

Some notes:
Accessing last element of a list in python:
A negative index accesses elements from the end of the list counting backwards. The last element of any non-empty list is always list[-1].
 
 


Python - Remove duplicate lines from file
Objective : Remove duplicate lines from a file (print first occurrence) which appeared exactly twice.

Input file:

$ cat file.txt
begin
ip 172.17.4.53
line 172.17.4.52
pl 172.17.4.51
pl 172.17.4.51
new 172.17.4.52
line 172.17.4.52
pl 172.17.4.51
end

Required: Remove duplicate lines from the above file i.e. print only the first occurrence of the lines which appeared exactly twice and for lines those appear more than twice or appeared only once, no action required.

i.e. Required output should look like this:

begin
ip 172.17.4.53
line 172.17.4.52
pl 172.17.4.51
pl 172.17.4.51
new 172.17.4.52
pl 172.17.4.51
end

The python script 'remove-duplicate.py' :

d = {}

fp = open("file.txt.nodup","w")
text_file = open("file.txt", "r")
lines = text_file.readlines()
for line in lines:
    if not line in d.keys():
        d[line] = 0
    d[line] = d[line] + 1

for line in lines:
    if d[line] == 0:
        continue
    elif d[line] == 2:
        fp.write(line)
        d[line] = 0
    else:
        fp.write(line)

Executing it:

$ python remove-duplicate.py
$ cat file.txt.nodup
begin
ip 172.17.4.53
line 172.17.4.52
pl 172.17.4.51
pl 172.17.4.51
new 172.17.4.52
pl 172.17.4.51
end
 
 
Simple python file lookup function for newbie
Config file 'ip-mapping.txt' is a file of the following format:

$ cat /home/testusr/work/ip-mapping.txt
#id:ip1,ip2,ip3
200:172.17.4.12,172.17.4.14,172.17.4.10
205:172.17.4.14,172.17.4.14,172.17.4.11
210:172.17.4.12,172.17.4.18,172.17.4.18
208:172.17.4.11,172.17.4.10,172.17.4.19

Required: Create a simple python function which will accept an 'id' and will return 'ip1' from the list of ips.

The python script:

import os,sys

config = '/home/testusr/work/ip-mapping.txt'
if not os.path.exists(config):
    print config+' file not present'
    sys.exit()

def getip(id):
   all = open(config).readlines()
   for line in all:
        if line.startswith('#'):
            continue
        f=line.split(":")
        if f[0]==id:
                return f[1].split(',')[0]

ip=getip('205')
print ip

Executing it:

$ python get-ip.py
172.17.4.14

I am sure there will be much better solutions to this problem, please comment, really appreciated.

The description about Exit function of 'sys' module (source) :

sys.exit([arg])
Exit from Python. This is implemented by raising the SystemExit exception, so cleanup actions
specified by finally clauses of try statements are honored, and it is possible to intercept the exit
attempt at an outer level. The optional argument arg can be an integer giving the exit status
(defaulting to zero), or another type of object. If it is an integer, zero is considered “successful
termination” and any nonzero value is considered “abnormal termination” by shells and the
like. Most systems require it to be in the range 0-127, and produce undefined results otherwise.

Some systems have a convention for assigning specific meanings to specific exit codes, but these
are generally underdeveloped; Unix programs generally use 2 for command line syntax errors
and 1 for all other kind of errors. If another type of object is passed, None is equivalent to
passing zero, and any other object is printed to sys.stderr and results in an exit code of 1. In
particular, sys.exit("some error message") is a quick way to exit a program when an error occurs.


Python - count instances without a specific line
Input file:

$ cat data.txt
k:begin:0
i:0:66
i:1:76
t:1:143
k:end:0
k:begin:7
i:0:55
i:1:65
i:2:57
k:end:7
k:begin:2
i:0:10
i:1:0
t:1:10
k:end:7
k:begin:2
i:0:46
t:0:46
k:end:7
k:begin:9
i:0:66
i:1:56
i:2:46
i:3:26
k:end:7

Required: Count total number of instances (one instance being from a 'k:begin' to 'k:end' line) which do not have a 't' line associated.

The python program:

import sys
count=0
data = open(sys.argv[1]).readlines()
for i in range(len(data)):
       if data[i].startswith("k:end") and data[i-1].split(":")[0]!="t":
               count=count+1
print count

Executing it:

$ python count_no_t.py data.txt
2


Python convert string to tuple & list
Let's check the use of python 'tuple' and 'list' in-built functions.

tuple([iterable]) 

It returns a 'tuple' whose items are the same and in the same order as iterable‘s items. iterable may be a sequence, a container that supports iteration, or an iterator object.

tuple('xyz') returns ('x', 'y', 'z') and tuple([1, 2, 3]) returns (1, 2, 3)

e.g.

$ cat file.txt
Python Prog
Readline
Programming

Now:

>>> for line in open("file.txt"):
...     t = tuple(line)
...     print t
...
('P', 'y', 't', 'h', 'o', 'n', ' ', 'P', 'r', 'o', 'g', '\n')
('R', 'e', 'a', 'd', 'l', 'i', 'n', 'e', '\n')
('P', 'r', 'o', 'g', 'r', 'a', 'm', 'm', 'i', 'n', 'g', '\n')
>>>


list([iterable])

It returns a list whose items are the same and in the same order as iterable‘s items. iterable may be either a sequence, a container that supports iteration, or an iterator object. If iterable is already a list, a copy is made and returned, similar to iterable[:]. For instance, list('xyz') returns ['x', 'y', 'z'] and list( (1, 2, 3) ) returns [1, 2, 3].

>>>
>>> for line in open("file.txt"):
...     l = list(line)
...     print l
...
['P', 'y', 't', 'h', 'o', 'n', ' ', 'P', 'r', 'o', 'g', '\n']
['R', 'e', 'a', 'd', 'l', 'i', 'n', 'e', '\n']
['P', 'r', 'o', 'g', 'r', 'a', 'm', 'm', 'i', 'n', 'g', '\n']
>>>
 
 
Split a file into sub files in python
Input file 'file.txt' is basically a log file containing running information of certain device interfaces in the following format:

$ cat file.txt
debug: on
max allowed connection: 3
tr#45
Starting: interface 78e23
Fan Status: On
Speed: -
sl no: 3431212-2323-90
vendor: aledaia
Stopping: interface 78e23
tr#90
newdebug received
Starting: interface 78e24
Fan Status: Off
Speed: 5670
sl no: 3431212-2323-90
vendor: aledaia
Stopping: interface 78e24
Starting: interface 68e73
Fan Status: On
Speed: 1200
sl no: 3431212-2323-90
vendor: aledaia
Stopping: interface 68e73
tr#99

Required:

Split the above file into sub-files such that
- Each sub file conatins information of an interface (basically information from 'Starting' and 'Stopping' of the interface)
- Sub-file name should be of the format: interface-name_someSLno.txt

The python script:

flag=0;c=0
for line in open("file.txt"):
    line=line.strip()
    if line.startswith("Stopping"):
        flag=0
        o.close()
    if line.startswith("Starting"):
        interface=line.split(" ")[2]
        flag=1;c=c+1
        o=open(interface+"_"+str(c)+".txt","w")
    if flag and not line.startswith("Starting"):
        print >>o, line

Output:

$ cat 78e23_1.txt
Fan Status: On
Speed: -
sl no: 3431212-2323-90
vendor: aledaia

$ cat 78e24_2.txt
Fan Status: Off
Speed: 5670
sl no: 3431212-2323-90
vendor: aledaia

$ cat 68e73_3.txt
Fan Status: On
Speed: 1200
sl no: 3431212-2323-90
vendor: aledaia
 
Python - print last few characters
Input file:

$ cat file.txt
sldadop233masdsa213313131ada121
sltadop233masdsa813313133cso128
slyadop233masdsa11331313Kada134
slqadop233masdsa31331313tada162


Required: Print last 6 characters of each line of the above input file.

The python script:

$ cat extract-last.py
import sys
for line in sys.stdin:
    print '%s' % (line[-7:-1])

Executing it:

$ python extract-last.py < file.txt
ada121
cso128
ada134
ada162

Things to learn:
- How to read a file in python from stdin

Other alternatives in UNIX are:

#Using bash parameter substitution
$ while read line ; do echo ${line: -6}; done < file.txt

#Since all lines are of fixed length, we can use 'cut' command
$ cut -c26-31 file.txt

#Using sed
$ sed 's/^.*\(......\)$/\1/' file.txt

#Using awk
$ awk '{ print substr( $0, length($0) - 5, length($0) ) }'  file.txt
 
Remove all except digits using python
Input file:

$ cat file.txt
4590:21333 2ewwq13232
12ada1212w1 1
13224 9#09io#
qw2323000 9023

Required: From the above file only keep the digits (i.e. remove all other characters except digits)

Way1: Using python Regular Expression special character '\D' which matches any non-digit character (equivalent to the set [^0-9])

$ python
Python 2.5.2 (r252:60911, Jul 22 2009, 15:35:03)
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu3)] on linux2
>>> import re
>>> for line in open('file.txt'):
...     re.sub("\D", "",line)
...
'459021333213232'
'12121211'
'13224909'
'23230009023'
>>>

Another way : Using python filter built-in function to iterate isdigit() on all lines of the file.

>>>
>>> for line in open('file.txt'):
...     filter(lambda x: x.isdigit(), line)
...
'459021333213232'
'12121211'
'13224909'
'23230009023'
>>>
 
Change file delimiter using Python
Input file is comma delimited:

$ cat /tmp/file.txt
5232,92338,84545,34,
2233,25644,23233,23,
6211,1212,4343,434,
2434,621171,9121,33,


Required:

Convert the above comma(,) delimited file to a colon(:) delimited file such that there is no colon at the end of each line.

Python solution:

$ python
Python 2.5.2 (r252:60911, Jul 22 2009, 15:35:03)
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu3)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> fp = open("/tmp/file.txt.new","w")
>>> for line in open('/tmp/file.txt'):
...     fp.write(line.strip()[:-1].replace(',',':')+'\n')
...
>>>

Output:

$ cat /tmp/file.txt.new
5232:92338:84545:34
2233:25644:23233:23
6211:1212:4343:434
2434:621171:9121:33

Alternative solutions:

An alternative using UNIX sed will be:

$ sed -e 's/,/:/g' -e 's/:$//g' /tmp/file.txt


Print line next to pattern in python
Input file: 'file.txt' contains results of a set of students in the following format (i.e. for any student result precedes the student id)

$ cat file.txt
Result:Pass
id:502
Result:Fail
id:909
Result:Pass
id:503
Result:Pass
id:501
Result:Fail
id:802

Required: Print the Ids of the students who have passed the exam.

The python program:

fp = open("passedids.txt","w")
data = open("file.txt").readlines()
for i in range(len(data)):
        if data[i].startswith("Result:Pass"):
                fp.write(data[i+1].split(":")[1])

Executing it:

$ python printnext.py
$ cat passedids.txt
502
503
501

Another python alternative:

fp=open('file.txt','r')
previous_line = ""

for current_line in fp:
    if 'Result:Pass' in previous_line:
        print current_line.split(":")[1],
    previous_line = current_line
fp.close()

Executing it:

$ python printnext1.py
502
503
501


Python - print section of file using line number
e.g. Print the section of input file 'input.txt' between line number 22 and 89.

Using python enumerate function sequence numbers:

for i,line in enumerate(open("file.txt")):
    if i >= 21 and i < 89 :
        print line,

And if you want to write the section to a new file say '/tmp/fileA'

fp = open("/tmp/fileA","w")
for i,line in enumerate(open("file.txt")):
    if i >= 21 and i < 89 :
        fp.write(line)

Another approach:

print(''.join(open('file.txt', 'r').readlines()[21:89])),

And if you wish to write the section to a new file say '/tmp/fileB'

fp = open("/tmp/fileB","w")
fp.write(''.join(open('file.txt', 'r').readlines()[21:89])),

Read about python enumerate function here and below is a small example using python enumerate function:

>>> for i, student in enumerate(['Alex', 'Ryan', 'Deb']):
...     print i, student
...
0 Alex
1 Ryan
2 Deb
>>>



 
Python - Adding numbers in a list
Lets see some of ways in python to add the numbers present in a list.

Suppose:

>>> numlist = [10,20,5,30]
>>> numlist
[10, 20, 5, 30]
>>> print sum(numlist)
65

Using python built in function 'reduce'

>>> numlist
[10, 20, 5, 30]
>>> def add(x, y): return x + y
...
>>> sum = reduce(add, numlist)
>>> sum
65

Enhancing the above using python 'lambda' function

>>> numlist
[10, 20, 5, 30]
>>> reduce(lambda b,a: a+b, numlist)
65
>>>

Or using python for loop:

>>> numlist
[10, 20, 5, 30]
>>> sum = 0
>>> for i in numlist:
...     sum += i
...
>>> sum
65
>>>
Python - time difference between dates
Required: 

Find the time difference between two dates (of following format) in seconds and in hh:mm:ss format.

e.g.

date1='Oct/09/2009 10:58:01' and
date2='Oct/10/2009 12:17:10'

find the difference between date1 and date2 in seconds(i.e. 91149 seconds) and later convert it to hh:mm:ss format (i.e. 25:19:09).

The complete python program:

import sys,time,string,getopt

def usage():
    print "Usage: adbtimediff.py -f <fromTime> -t <toTime> \n"
    sys.exit(2)


def parse_args():
    global fromTime,toTime
    fromTime = toTime = ""

    try:
        opts, args = getopt.getopt(sys.argv[1:], "f:t:", ["fromtime", "totime"])
    except getopt.GetoptError:
        print "Invalid arguments, exiting"
        sys.exit(2)

    for arg, val in opts:
        if  arg in ("-f","--fromtime"):
            fromTime = val
        elif arg in ("-t","--totime"):
            toTime = val

    if fromTime == toTime == "" :
        usage()

def compute_time(time1):
    t = time1.split(':')
    return time.mktime(time.strptime(":".join(t[0:len(t)]),"%b/%d/%Y %H:%M:%S"))

def subtract(list):
    return list[1] - list[0]

def time_convert(secs):
    secs = int(secs)
    mins = secs // 60
    hrs = mins // 60
    return "%02d:%02d:%02d" % (hrs, mins % 60, secs % 60)

def main():
    parse_args()
    print "Fromtime : " + str(fromTime) + '\n' + "Totime : " + str(toTime)
    timelist = [ fromTime, toTime ]
    s = map(compute_time,timelist)
    d = subtract(s)
    print "diff in seconds : " + str(d)
    f = str(d).split('.')
    final = time_convert(f[0])
    print "Total difference in required format : " + str(final)

main()


Executing the above script:

$ python timediff.py -f 'Oct/09/2009  10:58:01' -t 'Oct/10/2009  12:17:10'

Output:

Fromtime : Oct/09/2009  10:58:01
Totime : Oct/10/2009  12:17:10
diff in seconds : 91149.0
Total difference in required format : 25:19:09


Python - seconds to hh-mm-ss conversion
Solution1: Using python 'time' module strftime function.
 
Python 2.5.2 (r252:60911, Jul 22 2009, 15:35:03)
[GCC 4.2.4 (Ubuntu 4.2.4-1ubuntu3)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import time
>>> time.strftime('%H:%M:%S', time.gmtime(7302))
'02:01:42'
>>> time.strftime('%H:%M:%S', time.gmtime(86399))
'23:59:59'
>>> time.strftime('%H:%M:%S', time.gmtime(86405))
'00:00:05'

So as seen above this solution works only for num seconds < 1 day (86400 seconds)

Solution2: Using python datetime module, timedelta object.

>>> import datetime
>>> x = datetime.timedelta(seconds=7302)
>>> str(x)
'2:01:42'
>>> x = datetime.timedelta(seconds=86399)
>>> str(x)
'23:59:59'
>>> x = datetime.timedelta(seconds=86405)
>>> str(x)
'1 day, 0:00:05'

Solution3: Using normal division in python

import sys

secs = int(sys.argv[1])
mins = secs // 60
hrs = mins // 60

#hh:mm:ss
print "%02d:%02d:%02d" % (hrs, mins % 60, secs % 60)

#mm:ss
print "%02d:%02d" % (mins, secs % 60)

Executing it:

$ python timeconv.py 7302
02:01:42
121:42

$ python timeconv.py 86399
23:59:59
1439:59

$ python timeconv.py 86405
24:00:05
1440:05
 
Python - delete lines between two pattern
Input file:

$ cat input.txt
test1
test2
test3
BEGIN
test4
test5
test6
END
test7
test8
test9
BEGIN
test10
test11
END
test12

Required:
From the above file delete the lines which are between a BEGIN-END block and print rest of the lines.

The python script deleteline.py:

flag = 1
linelist = open("input.txt").readlines()
for line in linelist:
    if line.startswith("BEGIN"):
        flag = 0
    if line.startswith("END"):
        flag = 1
    if flag and not line.startswith("END"):
       print line,

Executing it:

$ python deleteline.py
test1
test2
test3
test7
test8
test9
test12

Now if we need to print the lines which are between a BEGIN-END block.
Here is a modification of the above scirpt.

flag = 1
linelist = open("input.txt").readlines()
for line in linelist:
    if line.startswith("BEGIN"):
        flag = 0
    if line.startswith("END"):
        flag = 1
    if not flag and not line.startswith("BEGIN"):
       print line,

Executing it:

$ python printline.py
test4
test5
test6
test10
test11


Python string methods for case conversion
Few important python string methods for case conversion.

swapcase()
Return a copy of the string with uppercase characters converted to lowercase and vice versa.

upper()
Return a copy of the string converted to uppercase.

title()
Return a titlecased version of the string: words start with uppercase characters, all remaining cased characters are lowercase.

lower()
Return a copy of the string converted to lowercase.

capitalize( )
Return a copy of the string with only its first character capitalized.

On python prompt:

>>> s='www.ExAmple.cOM'
>>> s
'www.ExAmple.cOM'
>>> s.swapcase()
'WWW.eXaMPLE.Com'
>>> s.upper()
'WWW.EXAMPLE.COM'
>>> s.lower()
'www.example.com'
>>> s.title()
'Www.Example.Com'
>>> s.capitalize()
'Www.example.com'
>>> st="This is the Best"
>>> st.capitalize()
'This is the best'
>>> st.title()
'This Is The Best'
 
Find text string in file in Python
Each line of file "querymapping.txt" contains two fields.
1st field is a sql query and
2nd one is a filename where the output of that sql query is stored.

$ cat querymapping.txt
select * from tab_fan_details;|/tmp/query7
select * from tab_fan_speed_details;|/tmp/query4
select * from tab_fan_spec;|/tmp/query1

Required:

Write a python function to look-up a particular sql query (send as 1st argument to the script ) in the querymapping.txt file and return the query output filename. If no match found return default filename as '/tmp/query0'

The python program:

import sys
default='/tmp/query0'

def lookupfilename(query):
    '''Lookup query output filename from query'''
    for line in open('querymapping.txt'):
        if query.strip() in line.strip().split("|")[0]:
            return line.strip().split("|")[1]
    return default

fname=lookupfilename(sys.argv[1])
print fname

Executing the script:

$ python qlookup.py "select * from tab_fan_speed_details;"
/tmp/query4

$ python qlookup.py "select * from tab_fan_speed_details_new;"
/tmp/query0


Print file content to output - Python
Required: Write a python program to print the content of a file to output (same as Linux/UNIX cat command do)

Way1: Using file.read file object in python

import sys,os.path

if len(sys.argv) < 2:
   print 'No file specified'
   sys.exit()
else:
   try:
      f = open(sys.argv[1], 'r')
      print f.read(),
      f.close()
   except IOError:
      print "File" + sys.argv[1] + "does not exist."


Execute it this way: To print the contents of file.txt to the output.

$ python cat-read.py file.txt


Way2: Another similar python program using file.readline

import sys

def readfile(fname):
   f = file(fname)
   while True:
      line = f.readline()
      if len(line) == 0:
         break
      print line.strip() #Avoid strip: print line,
   f.close()

if len(sys.argv) < 2:
   print 'No file specified'
   sys.exit()
else:
   readfile(sys.argv[1])

