---
title: "Perl5"
date: 2018-12-28T23:38:24+09:00
draft: false
tags: [Perl 5]
---

tags:  Perl 5

It's been a while since I could enjoy daily programming life with Perl 5.  
I tried to find a chance to make any program with Perl, but it's not easy especially when co-workers does not looks very happy reading the Perl code.  
There are many languages showing off their fancy features and paradigm, like Kotlin and Haskell or even [new projects on going with OpenJDK](https://openjdk.java.net/projects/), however I believe perl is still a very powerful tool for any kind of utilization.  
The purpose of posts regarding the perl is mostly like a memo to myself.  

# perldoc
- Extension of perldoc(POD: Plain Old Documentation) is often .pod or .pm(perl module)
- .pm extension is used if .pm file has embedded POD and the .pod is not found.  
- Source code of the module is also visible from perldoc
```bash
perldoc -m MeCab
perldoc -m Try::Tiny
```

## Documents structure
perl
perlfaq
perltoc
perldiag
perldoc CGI

## useful switchs
-D : Describes search for the item in detail
-q : `perldoc -q "something I'm looking for"` perlfaq-search-regexp
-l : `perldoc -l Time::HiRes` Display only the file name of the module found
Type `> perldoc perldoc` for details

## shebang lines
The third line of below code block is to tell perl to use __env__ program to find out which perl is set as the default perl on the system.  
```perl
#!/usr/bin/perl
#!/usr/local/bin/perl
#!/usr/bin/env perl 
```

# Module
- The perl finds module by looking through the dirctories in the __@INC__, which contains __module search path__
```bash
$ perl -V
$ perl -le "print for @INC"
perldoc -l Time::HiRes # see location
perl -MTime::HiRes -e 'print $INC{"Time/HiRes.pm"}.\n'
```

## Module Path
### Set path in program
Following code doesn't work becuase `unshift` is executed at runtime and the `use` pragma evaluated at compile time.  
```perl
unshift @INC, '/Users/wonha/lib';
use Navigation::SeatOfPandts;
```
Correct way is to use BEGIN block : 
```perl
BEGIN {unshift @INC, '/Users/gilligan/lib'; }
use Navigation::SeatOfPandts;
```

# Context
- It is __comma operator__ impliciting the list context, not paranthese.   
- But in order to make empty list or one-element list, use paranthese, not comma
```perl
($dino) = something;
@arr = ( );
```

# Scoping
- `my` keyword declares __lexical variable__, which is same as __private varibale__ of that scope  
- `state` is similar to `final` keyword in Java
```perl
use feature 'state'
```

# Special Variables
```bash
> perldoc perlvar
```

VARIABLE | DESCRIPTION
--- | ---
 @\_ | a
@INC | Contains paths to look for files loaded with _do, require, or use_.
%INC | Contains entries for every file loaded with _do, require, or use_.
$^V | The current Perl version
$^X | The executable used to execute your program
$@ | Perl syntax error trapped by _eval_.
$/ | input record separator (defaultly, newline)
$\ | output record separator
$" | list separator ( print "@array\n";)
$! | contain system call failure reason
$l | set this to 1 will set the currently selected filehande(by select operator) buffer flushed after each output operation
%ENV | .
$$ | current process ID

# String
- **" ", , , qq{}**: escape and interpolate have full power.   
- **' ', qw//, q{}**: escape single quote(or used delimiter) and backslach.   
```perl
my $letter = <<"SQL";
SELECT c.id
	FROM customers c
	JOIN orders o On c.id = customer_id
SQL
```

## Unicode

# Basic control flow with Hash & Array
```perl
my %french_for = qw/1 un 2 deux 3 trois/;

###

my %perl = map { $_, 1 } @required;
for my $item (@required) {
	if ( $perl{$item} ) {
		print "perl have the item\n";
	}
}

###

@arry = %people = ( %classmate, austen => 'Jane', Lincoln => "Abraham" );

### 

my(undef, undef, undef, undef, $mtime, $ctime) = stat $some_file # this is annoying
my ($ctime1, $mtime, $ctime2) = (stat $some_file)[5,4,5]; # use a list slice

###

while ( ( $index, $value ) = each @rocks ) {
	print "$index: $value\n";
}

###

my @fields = split /:/, ":::a:b:c:::"; # gives ("", "", "", "a", "b", "c")
```

# Caching recursion : Memoiz
- tail call recursion : goto
- caching previous result during recursive call : memoize module
```perl
use Memoize;
memoize('F');

sub F {
	my $n = shift;
	return 0 if $n == 0;
	return 1 if $n == 1;
	return F($n-1) + F($n-2);
}
print F(7);
```