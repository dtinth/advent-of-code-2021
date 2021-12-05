# Advent of Code 2021

## My workflow

I have in the repo `work.rb` and `input.txt`. I open the project in VS Code and hit Cmd+Shift+B. This will trigger the [build task](./vscode/tasks.json) that runs a file watcher (`npm install --global onchange`) which will run the code whenever I save.


## Legend

- 🏌️ Code golfing

## Solutions

### [Day 1: Sonar Sweep](https://www.reddit.com/r/adventofcode/comments/r66vow/2021_day_1_solutions/) 🏌️

<details><summary>See code</summary>

```ruby
# Ruby, 1381 / 452
p $<.read.split.map(&:to_i).each_cons(2).count{|a,b|b>a}
p $<.read.split.map(&:to_i).each_cons(3).map(&:sum).each_cons(2).count{|a,b|b>a}
```

Improvements with [Reddit suggestions by u/gurgeous, u/BluFoot, u/442401](https://www.reddit.com/r/adventofcode/comments/r66vow/2021_day_1_solutions/hmrf5u4/?utm_source=reddit&utm_medium=web2x&context=3):

```ruby
p $<.each_cons(2).count{_2.to_i>_1.to_i}
p $<.map(&:to_i).each_cons(3).each_cons(2).count{_2.sum>_1.sum}
```

</details>

### [Day 2: Dive!](https://www.reddit.com/r/adventofcode/comments/r6zd93/2021_day_2_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 251 / 155
xx = 0
y1 = 0
y = 0
aim = 0
$<.each do |x|
  c = x.split
  v = c[1].to_i
  case c[0]
  when "forward"
    xx += v
    y += aim * v
  when "down"
    y1 += v
    aim += v
  when "up"
    y1 -= v
    aim -= v
  end
end

p xx*y1
p xx*y
```

</details>

### [Day 3: Dive!](https://www.reddit.com/r/adventofcode/comments/r7r0ff/2021_day_3_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 28 / 79
report = $<.to_a.map(&:strip).map(&:chars)

# Part 1
g = report.transpose.map { |x| x.group_by{|y|y}.sort_by{_2.length}[-1][0] }.join.to_i(2)
e = report.transpose.map { |x| x.group_by{|y|y}.sort_by{_2.length}[0][0] }.join.to_i(2)
p [g,e,g*e]

# Part 2
og = report
co = report
og[0].each_with_index do |bit, i|
  mcbs = og.transpose.map { |x| sr = x.group_by{|y|y}.sort_by{[_2.length,_1.to_i]}[-1][0] }
  og = og.filter { |x| x[i] == mcbs[i] }
  break if og.length == 1
end
og[0].each_with_index do |bit, i|
  lcbs = co.transpose.map { |x| sr = x.group_by{|y|y}.sort_by{[_2.length,_1.to_i]}[0][0] }
  co = co.filter { |x| x[i] == lcbs[i] }
  break if co.length == 1
end
og = og[0].join.to_i 2
co = co[0].join.to_i 2
p [og,co,og*co]
```

</details>

### [Day 4: Giant Squid](https://www.reddit.com/r/adventofcode/comments/r8i1lq/2021_day_4_solutions/)

<details><summary>See code</summary>

```ruby
numbers = gets.split(',').map(&:to_i)

class Board
  def initialize(data)
    @data = data
    @basis = (data + data.transpose)
  end
  def win?(numbers)
    @basis.any? { |x| (numbers & x).size == 5 }
  end
  def data
    @data
  end
end

boards = $<.read.split(/\n\s*\n/).map { _1.lines.map { |x| x.split.map(&:to_i) }.reject(&:empty?) }.map { |x| Board.new(x) }
last_win = 0
(0..numbers.length).each do |c|
  selected_numbers = numbers[0..c]
  winning = boards.filter { _1.win?(selected_numbers) }
  if winning.length > 0
    p winning.map { |x| (x.data.flatten - selected_numbers).sum * numbers[c] }
    boards -= winning
  end
end
```

</details>