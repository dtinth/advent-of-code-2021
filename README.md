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

### [Day 5: Hydrothermal Venture](https://www.reddit.com/r/adventofcode/comments/r9824c/2021_day_5_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 43 / 66
data = $<.map { |line|
  line.split('->').map { |part|
    part.split(',').map(&:to_i)
  }
}

o = Hash.new(0)
data.each do |(x1, y1), (x2, y2)|
  a, b = [x1, x2].sort
  c, d = [y1, y2].sort
  if a == b || c == d
    (a..b).each { |x|
      (c..d).each { |y|
        o[[x, y]] += 1
      }
    }
  else
    # Comment out this block for Part 1.
    o[[x1, y1]] += 1
    while x1 < x2
      if y1 < y2
        y1 += 1
      else
        y1 -= 1
      end
      x1 += 1
      o[[x1, y1]] += 1
    end
    while x1 > x2
      if y1 < y2
        y1 += 1
      else
        y1 -= 1
      end
      x1 -= 1
      o[[x1, y1]] += 1
    end
  end
end

(0..10).each { |y|
  (0..10).each { |x|
    print (o[[x, y]] >= 1 ? o[[x,y]].to_s : '.')
  }
  puts
}

p o.values.count { |x| x > 1 }
```

</details>

### [Day 6: Lanternfish](https://www.reddit.com/r/adventofcode/comments/r9z49j/2021_day_6_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 1337 / 528
# (Cleaned up)
fishes = gets.split(',').map { _1.to_i }.tally
256.times do
  next_fishes = Hash.new(0)
  fishes.each do |k, f|
    if k == 0
      next_fishes[8] += f
      next_fishes[6] += f
    else
      next_fishes[k - 1] += f
    end
  end
  fishes = next_fishes
end
p fishes.values.sum
```

</details>

### [Day 7: The Treachery of Whales](https://www.reddit.com/r/adventofcode/comments/rar7ty/2021_day_7_solutions/)

<details><summary>See code</summary>

```ruby
crabs = gets.split(',').map(&:to_i)
p (0..1000).map { |u| crabs.sum { |x| (x-u).abs } }.min
p (0..1000).map { |u| crabs.sum { |x| n = (x-u).abs; (n*(n+1))/2 } }.min
```

</details>

### [Day 8: Seven Segment Search ](https://www.reddit.com/r/adventofcode/comments/rbj87a/2021_day_8_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 307 / 38
segments = [ 'abcefg', 'cf', 'acdeg', 'acdfg', 'bcdf', 'abdfg', 'abdefg', 'acf', 'abcdefg', 'abcdfg' ]
data = [*$<].join.gsub(/\|\s+/, "| ").lines

# Part 1
p data.flat_map { _1.split('|').last.split }.map { _1.chars.sort.join }.count { [1, 4, 7, 8].map { |x| segments[x].length }.include?(_1.length) }

# Part 2
sum = 0
data.each do |x|
    input, output = x.split('|').map(&:split)
    correct = 'abcdefg'.chars.permutation.find { |c|
        v = c.join
        input.map { |x| x.tr(v, 'abcdefg').chars.sort.join }.sort == segments.sort
    }.join
    mapping = segments.map { _1.tr('abcdefg', correct).chars.sort.join }
    result = output.map { mapping.index(_1.chars.sort.join) }.join.to_i
    sum += result
end
p sum
```

</details>

### [Day 9: Smoke Basin](https://www.reddit.com/r/adventofcode/comments/rca6vp/2021_day_9_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 67 / 55
map = $<.map {|x| x.chomp.chars.map{|c|c.to_i}}
w = map[0].length
h = map.length
pt = -> i,j {
    return 999 if i >= h || j >= w
    return 999 if i < 0 || j < 0
    return map[i][j]
}

# Part 1
sum = 0
(0...h).each do |i|
    (0...w).each do |j|
        low_point =
            pt[i,j] < pt[i+1,j] &&
            pt[i,j] < pt[i-1,j] &&
            pt[i,j] < pt[i,j+1] &&
            pt[i,j] < pt[i,j-1]
        sum += pt[i,j] + 1 if low_point
    end
end
p sum

# Part 2
visited = {}
basins = []
fill = -> i,j {
    basin = {size:0}
    basins << basin
    traverse = -> i,j {
        return unless pt[i,j] < 9
        return if visited[[i,j]]
        visited[[i,j]] = basin
        basin[:size] += 1
        traverse[i-1,j]
        traverse[i+1,j]
        traverse[i,j-1]
        traverse[i,j+1]
    }
    traverse[i,j]
}
(0...h).each do |i|
    (0...w).each do |j|
        fill[i,j]
    end
end
p basins.map { |b| b[:size] }.sort.last(3).inject(&:*)
```

</details>

### [Day 10: Syntax Scoring](https://www.reddit.com/r/adventofcode/comments/rd0s54/2021_day_10_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 333 / 176
illegal_score = {
    ')' => 3,
    ']' => 57,
    '}' => 1197,
    '>' => 25137,
}
matching = {
    '(' => ')',
    '<' => '>',
    '[' => ']',
    '{' => '}'
}

bad = 0
incomplete_lines = []
$<.each do |x|
    stack = []
    incorrect = false
    x.strip.chars.each do |y|
        if matching[y]
            stack << matching[y]
        elsif stack[-1] == y
            stack.pop
        else
            bad += illegal_score[y]
            incorrect = true
            break
        end 
    end
    incomplete_lines << stack if !incorrect
end
p bad

completion_score = {
    ')' => 1,
    ']' => 2,
    '}' => 3,
    '>' => 4,
}
scores = []
incomplete_lines.each do |stack|
    current = 0
    stack.reverse_each do |x|
        current *= 5
        current += completion_score[x]
    end
    scores << current
end
scores.sort!
p scores[scores.length / 2]
```

</details>

### [Day 11: Dumbo Octopus](https://www.reddit.com/r/adventofcode/comments/rds32p/2021_day_11_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 873/ 795
levels = [*$<].map(&:strip).map(&:chars).map { _1.map(&:to_i) }

p levels
coords = (0...levels.size).to_a.product((0...levels[0].size).to_a)
total_f = 0
show = -> { puts levels.map(&:join); puts }
update = -> {
    to_flash = {}
    try_flash = -> i, j {
        if i >= 0 && i < levels.size && j >= 0 && j < levels[0].size && !to_flash[[i,j]]
            levels[i][j] += 1
            if levels[i][j] > 9
                to_flash[[i,j]] = true
                total_f += 1
                try_flash[i-1, j-1]
                try_flash[i-1, j]
                try_flash[i-1, j+1]
                try_flash[i, j-1]
                try_flash[i, j+1]
                try_flash[i+1, j-1]
                try_flash[i+1, j]
                try_flash[i+1, j+1]
            end
        end
    }
    coords.each do |i, j|
        try_flash[i, j]
    end
    to_flash.each do |(i, j), _|
        levels[i][j] = 0
    end
}

# Part 1
p total_f

# Part 2
n = 0
loop {
    update[]
    n += 1
    p levels.flatten.uniq
    if levels.flatten.uniq == [0]
        p n
        break
    end
}
```

</details>

### [Day 12: Passage Pathing](https://www.reddit.com/r/adventofcode/comments/rehj2r/2021_day_12_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 61 / 524
graph = {}

$<.each do |a|
    f, t = a.strip.split('-')
    graph[f] ||= []
    graph[f] << t
    graph[t] ||= []
    graph[t] << f
end

list = []
paths = [['start']]

while paths.length > 0
    path = paths.shift
    current_node = path[-1]

    if current_node == 'end'
        list << path
        p list.length
        next
    end

    graph[current_node].each do |vertex|
        # Part 1: Change max_p to 1.
        max_p = path.uniq.filter{|a|a=~/^[a-z]+$/}.size == path.filter{|a|a=~/^[a-z]+$/}.size ? 2 : 1
        if vertex =~ /^[A-Z]+/ || (vertex != 'start' && path.count(vertex) < max_p)
            paths << path + [vertex]
        end
    end
end

p list.length
```

</details>

### [Day 13: Transparent Origami](https://www.reddit.com/r/adventofcode/comments/rf7onx/2021_day_13_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 46 / 10
inp = $<.read
points = inp.scan(/(\d+),(\d+)/).map { |crd| [crd[0].to_i, crd[1].to_i] }

plot = -> {
    h = Hash.new
    xs = []
    ys = []
    points.map do |x, y|
        h[[x, y]] = 1
        xs << x
        ys << y
    end
    (ys.min..ys.max).each do |y|
        (xs.min..xs.max).each do |x|
            print h[[x, y]] ? '#' : '.'
        end
        puts
    end
}

folds = inp.scan(/fold along (\w+)\=(\d+)/).map { |(axis, value)| [axis, value.to_i] }

p points.length

folds.each do |axis, value|
    points = points.map { |pt|
        if axis == 'x'
            x = pt[0] > value ? value - (pt[0] - value) : pt[0]
            [x, pt[1]]
        else
            y = pt[1] > value ? value - (pt[1] - value) : pt[1]
            [pt[0], y]
        end
    }
    points = points.uniq
    p points.length
end

plot[]
```

</details>

### [Day 14: Extended Polymerization](https://www.reddit.com/r/adventofcode/comments/rfzq6f/2021_day_14_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 45 / 60

template = gets.strip
rules = $<.read.scan(/^(\w\w) -> (\w)/)
rulebook = {}
rules.each { |pair| rulebook[pair[0]] = pair[1].upcase }
p template
p rulebook

part = 2
case part
when 1
    # Unoptimized
    10.times do
        template = template.chars.each_cons(2).map { |a, b| [a, rulebook[a+b]].join }.join + template.chars.last
    end
    t = template.chars.tally
    p t.values.max - t.values.min
when 2
    # This algorithm also works with part 1
    paircount = Hash.new(0)
    template.chars.each_cons(2) { |a, b| paircount[a + b] += 1 }
    40.times do
        old_pc = paircount.dup
        paircount = Hash.new(0)
        old_pc.each do |k, v|
            found = rulebook[k]
            paircount[k[0] + found] += v
            paircount[found + k[1]] += v
        end
    end
    t = Hash.new(0)
    paircount.each do |k, v|
        t[k[0]] += v
    end
    t[template[-1]] += 1
    p t.values.max - t.values.min
end
```

</details>

### [Day 15: Chiton](https://www.reddit.com/r/adventofcode/comments/rgqzt5/2021_day_15_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 26 / 1155
grid = $<.readlines.map { _1.strip.chars.map(&:to_i) }

# Part 1 - Wrong algorithm but right answer.
#          It assumes that you can only go down and right.
$cache = {}
min_risk_level = -> x, y {
    return 0 if x == 0 && y == 0
    return 99999999999 if x < 0 || y < 0
    $cache[[x,y]] ||= [min_risk_level[x-1,y], min_risk_level[x,y-1]].min + grid[y][x]
}
p min_risk_level[grid[0].length-1, grid.length-1]

# Part 2 - Brute force algorithm
grid = (0..4).flat_map { |n| grid.map { |r|
    (0..4).flat_map { |m| r.map { ((_1 + n + m) - 1) % 9 + 1 } }
} }
min = grid.map { _1.map { 999999 } }
min[0][0] = 0
cur_risk_level = -> a, x, y {
    return 999999 if x < 0 || y < 0 || x >= a[0].length || y >= a.length
    return a[y][x]
}
loop do
    next_min = min.each_with_index.map { |r, y|
        r.each_with_index.map { |c, x|
            inh = grid[y][x]
            [c,inh+cur_risk_level[min,x-1,y], inh+cur_risk_level[min,x+1,y], inh+cur_risk_level[min,x,y-1], inh+cur_risk_level[min,x,y+1]].min
        }
    }
    puts next_min.flatten.count(999999)
    break if next_min == min
    min = next_min
end
p cur_risk_level[min,grid[0].length-1, grid.length-1]

# Part 2 - A* algorithm. Requires `pqueue` gem.
require 'pqueue'
visited = {}
fringe = PQueue.new { |a, b| a.last < b.last }
fringe.push [0, 0, 0]
in_bounds = -> i, j { 0 <= i && i < grid.length && 0 <= j && j < grid[0].length }
last_v = 0

loop do
    first = fringe.pop
    break unless first

    i, j, v = first
    p [[i, j], v, fringe.length] if v > last_v
    last_v = v
    if [i, j] == [grid.length - 1, grid[i].length - 1]
        p v
        break
    end

    next if visited[[i, j]]
    visited[[i, j]] = true

    try_go = -> ii, jj {
        if in_bounds[ii, jj] && !visited[[ii, jj]]
            fringe.push [ii, jj, grid[ii][jj] + v]
        end
    }
    try_go[i + 1, j]
    try_go[i - 1, j]
    try_go[i, j + 1]
    try_go[i, j - 1]
end
```

</details>

### [Day 16: Packet Decoder](https://www.reddit.com/r/adventofcode/comments/rhj2hm/2021_day_16_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 106 / 100
input = gets.strip.chars.flat_map { |c| c.to_i(16).to_s(2).rjust(4,'0').chars.map(&:to_i) }
$versum = 0
p input
parse_packet = -> str, i, prefix, ptree {
    version = str[i...i+3].join.to_i(2)
    $versum += version
    puts "#{prefix}Version #{version} sum = #{$versum}"
    i += 3
    type = str[i...i+3].join.to_i(2)
    puts "#{prefix}Packet type = #{type}"
    i += 3
    ptree << version << type
    case type
    when 4
        vals = ''
        ended = false
        while !ended
            ended = str[i] == 0
            i += 1
            value = str[i...i+4].join
            puts "#{prefix}value = #{value}"
            vals += value
            i += 4
        end
        puts "#{prefix}>> = #{vals.to_i(2)}"
        ptree << vals.to_i(2)
    else
        length_type = str[i]
        i += 1
        case length_type
        when 0
            total_length = str[i...i+15].join.to_i(2)
            puts "#{prefix}Total length #{total_length}"
            i += 15
            end_pos = i + total_length
            while i < end_pos
                puts "#{prefix}. #{i}"
                subpacket = []
                ptree << subpacket
                i = parse_packet[str, i, prefix + '..', subpacket]
            end
            i = end_pos
        else
            n_subpackets = str[i...i+11].join.to_i(2)
            puts "#{prefix}Subpacket count #{n_subpackets}"
            i += 11
            n_subpackets.times do
                puts "#{prefix}. #{i}"
                subpacket = []
                ptree << subpacket
                i = parse_packet[str, i, prefix + '..', subpacket]
            end
        end
    end
    i
}

parse_tree = []
parse_packet[input, 0, "", parse_tree]

puts "Version sum: #{$versum}"

evaluate = -> a {
    version, type, *args = a
    return args[0] if type == 4
    case type
    when 0
        args.map(&evaluate).reduce(0, &:+)
    when 1
        args.map(&evaluate).reduce(1, &:*)
    when 2
        args.map(&evaluate).min
    when 3
        args.map(&evaluate).max
    when 5
        evaluate[args[0]] > evaluate[args[1]] ? 1 : 0
    when 6
        evaluate[args[0]] < evaluate[args[1]] ? 1 : 0
    when 7
        evaluate[args[0]] == evaluate[args[1]] ? 1 : 0
    else
        raise "Unknown type #{type}"
    end
}
p evaluate[parse_tree]
```

</details>



