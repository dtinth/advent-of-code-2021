# Advent of Code 2021

## My workflow

I have in the repo `work.rb` and `input.txt`. I open the project in VS Code and hit Cmd+Shift+B. This will trigger the [build task](./vscode/tasks.json) that runs a file watcher (`npm install --global onchange`) which will run the code whenever I save.

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

### [Day 2: Day 2: Dive!](https://www.reddit.com/r/adventofcode/comments/r6zd93/2021_day_2_solutions/)

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