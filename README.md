# Advent of Code 2021

## [Day 1: Sonar Sweep](https://www.reddit.com/r/adventofcode/comments/r66vow/2021_day_1_solutions/)

```ruby
# Ruby, 1381 / 452
p $<.read.split.map(&:to_i).each_cons(2).count{|a,b|b>a}
p $<.read.split.map(&:to_i).each_cons(3).map(&:sum).each_cons(2).count{|a,b|b>a}
```