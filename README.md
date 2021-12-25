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

### [Day 17: Trick Shot](https://www.reddit.com/r/adventofcode/comments/ri9kdq/2021_day_17_solutions/)

<details><summary>See code</summary>

```ruby
  # Ruby, 82 / 31
  target = gets.scan(/target area: x=([^,]+), y=([^,]+)/)[0].map { eval _1 }
  p target

  simulate = -> dx, dy {
      hit, x, y, y_max = false, 0, 0, 0
      loop do
          x += dx
          y += dy
          dx -= dx > 0 ? 1 : dx < 0 ? -1 : 0
          dy -= 1
          y_max = y if y > y_max
          if target[0].include?(x) && target[1].include?(y)
              hit = true
              break
          end
          break if y < target[1].min
      end
      [hit, x, y, y_max]
  }

  all_possibilities = (-100..400).to_a.product((-100..100).to_a).map { |dx, dy| simulate[dx, dy] }.filter { _1[0] }
  p all_possibilities.map { _1.last }.max
  p all_possibilities.count
```

</details>

### [Day 18: Snailfish](https://www.reddit.com/r/adventofcode/comments/rizw2c/2021_day_18_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 398 / 349
$g = 0
NumberNode = Struct.new(:value, :ident) do
    def inspect
        "#{value}"
    end
    def to_rb
        value
    end
    def mag
        value
    end
end
AdditionNode = Struct.new(:left, :right) do
    def inspect
        "[#{left.inspect}, #{right.inspect}]"
    end
    def to_rb
        [left.to_rb, right.to_rb]
    end
    def mag
        left.target.mag * 3 + 2 * right.target.mag
    end
end
Edge = Struct.new(:target) do
    def inspect
        "#{target.inspect}"
    end
    def to_rb
        target.to_rb
    end
end

to_node = -> data {
    if Array === data
        left = to_node[data.first]
        right = to_node[data.last]
        AdditionNode.new(Edge.new(left), Edge.new(right))
    else
        NumberNode.new(data, $g += 1)
    end
}

reduce = -> node {
    all_numbers = []
    traverse_numbers = -> n {
        if AdditionNode === n
            traverse_numbers[n.left.target]
            traverse_numbers[n.right.target]
        else
            all_numbers.push(n)
        end
    }
    traverse_numbers[node]
    done = false
    mode = 0
    traverse_action = -> n, depth = 0, edge {
        return if done
        if AdditionNode === n
            if depth >= 4 && mode == 0
                left_index = all_numbers.index(n.left.target)
                right_index = all_numbers.index(n.right.target)
                if left_index > 0
                    left = all_numbers[left_index - 1]
                    # p [:left, left.value, n.left]
                    left.value += n.left.target.value
                end
                # p [:all, all_numbers, left_index, right_index]
                right = all_numbers[right_index + 1]
                if right
                    # p [:right, right.value, n.right]
                    right.value += n.right.target.value
                end
                edge.target = NumberNode.new(0, $g += 1)
                done = true
            else
                traverse_action[n.left.target, depth + 1, n.left]
                traverse_action[n.right.target, depth + 1, n.right]
            end
        else
            if n.value >= 10 && mode == 1
                left_value = n.value / 2
                right_value = n.value - left_value
                left = NumberNode.new(left_value, $g += 1)
                right = NumberNode.new(right_value, $g += 1)
                child = AdditionNode.new(Edge.new(left), Edge.new(right))
                edge.target = child
                done = true
            end
        end
    }
    mode = 0
    traverse_action[node, 0, nil]
    mode = 1
    traverse_action[node, 0, nil]
    done
}

add = -> l, r {
    root = to_node[[l, r]]
    while reduce[root]
    end
    root.to_rb
}

input = $<.to_a.map { eval _1 }

# Part 1
p input.inject { |left, right|
    result = add[left, right]
    puts "  #{left.inspect}"
    puts "+ #{right.inspect}"
    puts "= #{result.inspect} (#{to_node[result].mag})"
    puts
    result
}.then { to_node[_1].mag }

# Part 2
p input.permutation(2).map { |left, right|
    result = add[left, right]
    [to_node[result].mag, result]
}.max
```

</details>

### [Day 19: Beacon Scanner](https://www.reddit.com/r/adventofcode/comments/rjpf7f/2021_day_19_solutions/)

<details><summary>See code</summary>

Ruby, 40 / 120

Part 1:

```ruby
require 'set'

$orientations = [0, 1, 2].permutation.to_a.flat_map { |a| [1, -1].repeated_permutation(3).to_a.map { |b| a.zip(b) } }
$gid = 100
Scanner = Struct.new(:index, :visible_points) do
    def find_overlap(another)
        visible_points.points.to_a.product(another.visible_points.points.to_a).each do |point1, point2|
            $orientations.each do |o|
                found_points = another.visible_points.transform_points(point2, point1, o)
                overlap = (found_points & visible_points.points)
                if overlap.size >= 12
                    return Scanner.new(($gid += 1), PointSet.new((found_points | visible_points.points).to_set))
                end
            end
        end
        return nil
    end
end

PointSet = Struct.new(:points) do
    def transform_points(pivot, target, mapping)
        map = -> point { mapping.map { point[_1] * _2 } }
        mapped_pivot = map[pivot]
        delta = target.zip(mapped_pivot).map { |tv, pv| tv - pv }
        translate = -> point { point.zip(delta).map { |a, b| a + b } }
        points.map { translate[map[_1]] }.to_set
    end
end

scanners = $<.read.split(/--- scanner (\d+) ---/).drop(1).each_slice(2).map {|a,b| Scanner.new(a.to_i, PointSet.new(b.split.map { _1.split(',').map(&:to_i) }.to_set)) }

loop do
    p scanners.length
    scanners.combination(2).to_a.each do |a, b|
        overlap = a.find_overlap(b)
        if overlap
            scanners.delete(a)
            scanners.delete(b)
            scanners << overlap
            break
        end
    end
    if scanners.length == 1
        p scanners[0].visible_points.points.count
        break
    end
end
```

Part 2a:

```ruby
require 'set'

$orientations = [0, 1, 2].permutation.to_a.flat_map { |a| [1, -1].repeated_permutation(3).to_a.map { |b| a.zip(b) } }
$gid = 100
Scanner = Struct.new(:index, :visible_points, :overlap_with) do
    def find_overlap(another)
        visible_points.points.to_a.product(another.visible_points.points.to_a).each do |point1, point2|
            $orientations.each do |o|
                transformer = another.visible_points.transformer(point2, point1, o)
                found_points = another.visible_points.transform_points(transformer)
                overlap = (found_points & visible_points.points)
                if overlap.size >= 12
                    puts "#{index} overlaps with #{another.index} size #{overlap.size}: #{point2} #{point1} #{o}"
                    return
                end
            end
        end
        return
    end
end

PointSet = Struct.new(:points) do
    def transformer(pivot, target, mapping)
        map = -> point { mapping.map { point[_1] * _2 } }
        mapped_pivot = map[pivot]
        delta = target.zip(mapped_pivot).map { |tv, pv| tv - pv }
        translate = -> point { point.zip(delta).map { |a, b| a + b } }
        -> point { translate[map[point]] }
    end
    def transform_points(transformer)
        points.map(&transformer).to_set
    end
end

scanners = $<.read.split(/--- scanner (\d+) ---/).drop(1).each_slice(2).map {|a,b| Scanner.new(a.to_i, PointSet.new(b.split.map { _1.split(',').map(&:to_i) }.to_set)) }
scanners.combination(2).to_a.each do |a, b|
    a.find_overlap(b)
    b.find_overlap(a)
end
puts "done"
```

Output from 2a is fed into 2b:

```ruby
Association = Struct.new(:from, :to, :pivot, :target, :orientation)

data = $<.read
    .scan(/(\d+) overlaps with (\d+) [^:]+: ([^\]]+\]) ([^\]]+\]) (.+)/)
    .map { Association.new(_1.to_i, _2.to_i, eval(_3), eval(_4), eval(_5)) }

def transformer(pivot, target, mapping)
    map = -> point { mapping.map { point[_1] * _2 } }
    mapped_pivot = map[pivot]
    delta = target.zip(mapped_pivot).map { |tv, pv| tv - pv }
    translate = -> point { point.zip(delta).map { |a, b| a + b } }
    -> point { translate[map[point]] }
end

get_pos = -> current, target, path {
    if current == target
        return [0,0,0]
    end
    visited = path.map(&:to)
    data.each do |association|
        next if association.from != current
        next if visited.include?(association.to)
        visited << association.to
        found = get_pos[association.to, target, path + [association]]
        if found
            xf = transformer(association.pivot, association.target, association.orientation)
            return xf[found]
        end
    end
    return nil
}

pos = (0..29).map do |i|
    scanner_pos = get_pos[0, i, []]
    puts "Scanner #{i}: #{get_pos[0, i, []]}"
    scanner_pos
end

p pos.combination(2).map { |a, b| a.zip(b).map { (_1 - _2).abs }.sum }.max
```

</details>

### [Day 20: Trench Map](https://www.reddit.com/r/adventofcode/comments/rkf5ek/2021_day_20_solutions/)

<details><summary>See code</summary>

```ruby
algorithm, input = $<.read.split(/\n\s*\n/)
algorithm = algorithm.split.join

light = {}
input.lines.each_with_index.map { |line, i| line.strip.chars.each_with_index.map { |char, j| light[[i, j]] = true if char == '#' } }

# (These values are added in after solving)
DEBUG = false
SHOW_PROGRESS = !DEBUG

show = -> m {
    min_i = -100
    min_j = -100
    max_i = 220
    max_j = 220
    count = 0
    (min_i..max_i).each do |i|
        p i if SHOW_PROGRESS
        (min_j..max_j).each do |j|
            print m[[i, j]] ? '#' : '.' if DEBUG
            count += 1 if m[[i, j]]
        end
        puts if DEBUG
    end
    p count
}

enhanced = -> m {
    cache = {}
    -> c {
        cache[c] ||= begin
            i, j = c
            val = 0
            (-1..1).each do |di|
                (-1..1).each do |dj|
                    val *= 2
                    val += 1 if m[[i + di, j + dj]]
                end
            end
            algorithm[val]
        end
        cache[c] == '#'
    }
}

# Part 1: Change to 2
2.times do
    light = enhanced[light]
end

show[light]
```

</details>

### [Day 21: Dirac Dice](https://www.reddit.com/r/adventofcode/comments/rl6p8y/2021_day_21_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 126 / 208

# Part 1
Player = Struct.new(:num, :score, :position)

p1 = Player.new(1, 0, 5)
p2 = Player.new(2, 0, 6)

$die = 0
$times = 0
roll = -> {
    $die += 1
    $times += 1
    $die = 1 if $die == 101
    return $die
}

roll3 = -> { roll[] + roll[] + roll[] }
players = [p1, p2]
loop {
    player = players.shift
    players << player
    move = roll3[]
    player.position += move
    player.position -= 1
    player.position %= 10
    player.position += 1
    player.score += player.position
    p [player]
    if player.score >= 1000
        p [players[0].score, $times, players[0].score * $times]
        break
    end
}

# Part 2
Player = Struct.new(:score, :position)
GameState = Struct.new(:turn, :players) do
    def next_states
        Enumerator.new do |y|
            [1, 2, 3].product([1, 2, 3], [1, 2, 3]).map(&:sum).each do |sum|
                y << GameState.new((turn + 1) % 2, players.each_with_index.map { |player, i|
                    i == turn ? player.dup.tap { |p|
                        p.position += sum
                        p.position -= 1
                        p.position %= 10
                        p.position += 1
                        p.score += p.position
                    } : player
                })
            end
        end
    end
    def ended?
        players.any? { |player| player.score >= 21 }
    end
end

initial_state = GameState.new(0, [Player.new(0, 5), Player.new(0, 6)])
all_states = {}
p1_win_states = []
p2_win_states = []

generate_states = -> current_state {
    return all_states[current_state] if all_states.key?(current_state)
    all_states[current_state] = Hash.new(0)
    if current_state.ended?
        p1_win_states << current_state if current_state.players[0].score >= 21
        p2_win_states << current_state if current_state.players[1].score >= 21
    else
        current_state.next_states.each do |next_state|
            generate_states[next_state][current_state] += 1
        end
    end
    all_states[current_state]
}

p :start
states = generate_states[initial_state]
p :finish

$cache = {}
p p1_win_states.count
p p2_win_states.count
p all_states.size
p :compute
count_paths = -> state {
    parents = generate_states[state]
    return 1 if parents.empty?
    $cache[state] ||= parents.sum { |parent, count| count_paths[parent] * count }
}

p p1_win_states.sum(&count_paths)
p p2_win_states.sum(&count_paths)
```

</details>

### [Day 22: Reactor Reboot](https://www.reddit.com/r/adventofcode/comments/rlxhmg/2021_day_22_solutions/hpix5qp/)

<details><summary>See code</summary>

```ruby
# Ruby, 145 / 45

# Part 1
data = $<.readlines.map { |l|
    command, xyz = l.split
    [command, xyz.split(',').map { |r| eval r.split('=').last }]
}

def loop_range(r, &x)
    b = [-50, r.begin].max
    e = [50, r.end].min
    if b < e
        (b..e).each(&x)
    else
        []
    end
end

on = {}

data.each do |cmd, range|
    case cmd
    when "on"
        xr, yr, zr = range
        loop_range xr do |x|
            loop_range yr do |y|
                loop_range zr do |z|
                    on[[x,y,z]] = 1
                end
            end
        end
    when "off"
        xr, yr, zr = range
        loop_range xr do |x|
            loop_range yr do |y|
                loop_range zr do |z|
                    on.delete([x,y,z])
                end
            end
        end
    end
end
p on.size

# Part 2
Cube = Struct.new(:x1, :x2, :y1, :y2, :z1, :z2) do
    def to_s
        "(#{x1}, #{x2}, #{y1}, #{y2}, #{z1}, #{z2})"
    end
    def valid?
        x1 < x2 && y1 < y2 && z1 < z2
    end
    def intersection(cube)
        Cube.new(
            [x1, cube.x1].max,
            [x2, cube.x2].min,
            [y1, cube.y1].max,
            [y2, cube.y2].min,
            [z1, cube.z1].max,
            [z2, cube.z2].min
        )
    end
    def intersect?(cube)
        intersection(cube).valid?
    end
    def volume
        (x2 - x1) * (y2 - y1) * (z2 - z1)
    end
end

data = $<.readlines.map { |l|
    command, xyz = l.split
    [command, Cube.new(*xyz.split(',').flat_map { |r| range = eval(r.split('=').last); [range.begin, range.end + 1] })]
}

cube_map = {}

fit_cube = -> new_cube, to_add {
    cube_map.keys.each do |cube|
        next unless new_cube.intersect?(cube)
        cube_map.delete(cube)
        xs = [cube.x1, cube.x2, new_cube.x1, new_cube.x2].uniq.sort
        ys = [cube.y1, cube.y2, new_cube.y1, new_cube.y2].uniq.sort
        zs = [cube.z1, cube.z2, new_cube.z1, new_cube.z2].uniq.sort
        xs.each_cons(2) do |x1, x2|
            ys.each_cons(2) do |y1, y2|
                zs.each_cons(2) do |z1, z2|
                    target_cube = Cube.new(x1, x2, y1, y2, z1, z2)
                    cube_map[target_cube] = true if to_add[target_cube, cube]
                end
            end
        end
    end
}

data.each do |cmd, new_cube|
    fit_cube[new_cube, -> slice, old_cube { old_cube.intersect?(slice) && !new_cube.intersect?(slice) }]
    cube_map[new_cube] = true if cmd == 'on'
end

p cube_map.keys.sum(&:volume)
```

</details>

<details><summary>See solution explanation</summary>

![image](https://user-images.githubusercontent.com/193136/147194806-e614d737-f631-4d34-b871-ec86dcc356d5.png)

</details>

### [Day 23: Amphipod](https://www.reddit.com/r/adventofcode/comments/rmnozs/2021_day_23_solutions/)

<details><summary>See code</summary>

```ruby
# Ruby, 613 / 24
require 'pqueue'

config = $<.read.scan(/\w/)

part = 2
config = [*config[0...4], *'DCBADBAC'.chars, *config[4...8]] if part == 2

$hallway_xs = [0, 1, 3, 5, 7, 9, 10]
$room_xs = [2, 4, 6, 8]
$target_xs = { 'A' => 2, 'B' => 4, 'C' => 6, 'D' => 8 }
$cost = { 'A' => 1, 'B' => 10, 'C' => 100, 'D' => 1000 }

Amphipod = Struct.new(:id, :target_letter, :x, :y) do
    def possible_movements(all_amphipods)
        if y > 0
            blocked = all_amphipods.any? { |o| o.id != id && o.x == x && o.y == y - 1 }
            return [] if blocked
            lefts = $hallway_xs.filter { |hx| hx < x && all_amphipods.none? { |o| o.id != id && o.y == 0 && o.x < x && o.x >= hx } }
            rights = $hallway_xs.filter { |hx| hx > x && all_amphipods.none? { |o| o.id != id && o.y == 0 && o.x > x && o.x <= hx } }
            [*lefts, *rights].map { |x| [x, 0] }
        else
            target_x = $target_xs[target_letter]
            between = [x, target_x].sort
            blocked = all_amphipods.any? { |o| o.id != id && o.y == 0 && between[0] < o.x && o.x < between[1] }
            return [] if blocked
            occupied_by_others = all_amphipods.any? { |o| o.id != id && o.y > 0 && o.target_letter != target_letter && o.x == target_x }
            return [] if occupied_by_others
            return [[target_x, 4]] if all_amphipods.none? { |o| o.x == target_x && o.y >= 1 }
            return [[target_x, 3]] if all_amphipods.none? { |o| o.x == target_x && o.y >= 1 && o.y <= 3 }
            return [[target_x, 2]] if all_amphipods.none? { |o| o.x == target_x && o.y >= 1 && o.y <= 2 }
            return [[target_x, 1]] if all_amphipods.none? { |o| o.x == target_x && o.y == 1 }
            return []
        end
    end
    def min_cost
        return 0 if x == $target_xs[target_letter]
        if y > 0
            return $cost[target_letter] * ((x - $target_xs[a.target_letter]).abs + 1)
        else
            return $cost[target_letter] * ((x - $target_xs[a.target_letter]).abs + 1 + y)
        end
    end
end

State = Struct.new(:amphipods, :energy, :depth) do
    def possible_moves
        next_states = []
        amphipods.each_with_index do |a, i|
            a.possible_movements(amphipods).each do |x, y|
                next_amphipods = amphipods.dup
                next_amphipods[i] = Amphipod.new(a.id, a.target_letter, x, y)
                consumption = $cost[a.target_letter] * ((x - a.x).abs + (y - a.y).abs)
                next_states << State.new(next_amphipods, energy + consumption, depth + 1)
            end
        end
        next_states
    end
    def finish?
        amphipods.all? { |a| a.x == $target_xs[a.target_letter] && a.y > 0 }
    end
    def min_cost
        @min_cost ||= energy + amphipods.sum { |a| $cost[a.target_letter] * (a.x - $target_xs[a.target_letter]).abs }
    end
end

state = State.new(config.each_with_index.map { |c, i| Amphipod.new(i, c, $room_xs[i % 4], i / 4 + 1) }, 0, 0)
fringe = PQueue.new { |a, b| a.min_cost < b.min_cost }
fringe.push(state)
visited = {}
last_time = Time.now

loop do
    first = fringe.pop
    break unless first
    next if visited[first.amphipods]
    visited[first.amphipods] = true
    if first.finish?
        puts first.energy
        break
    end
    if Time.now > last_time + 1.0
        p [visited.length, fringe.size, first.energy, first.min_cost, first.depth]
        last_time = Time.now
    end
    first.possible_moves.each { |s| fringe.push(s) }
end
```

</details>

### [Day 24: Arithmetic Logic Unit](https://www.reddit.com/r/adventofcode/comments/rnejv5/2021_day_24_solutions/)

[See write-up](https://notes.dt.in.th/20211224T121217Z7595)

### [Day 25: Sea Cucumber](https://www.reddit.com/r/adventofcode/comments/rnejv5/2021_day_24_solutions/)


<details><summary>See code</summary>

```ruby
# Ruby, 46 / 48
cukes = $<.readlines.map(&:strip).map(&:chars)
w = cukes.first.length
h = cukes.length

map = {}
cukes.each_with_index do |row, y|
    row.each_with_index do |c, x|
        map[[x, y]] = c if c != '.'
    end
end

move_target = -> k, v, mode, map {
    x, y = k
    proposed = case v
    when '>'
        [(x + 1) % w, y]
    when 'v'
        [x, (y + 1) % h]
    end
    map[proposed] || mode != v ? k : proposed
}

next_map = -> map, mode {
    new_map = {}
    map.each do |k, v|
        move_to = move_target[k, v, mode, map]
        new_map[move_to] = v
    end
    new_map
}

i = 0
loop do
    c = next_map[next_map[map, '>'], 'v']
    i += 1
    puts i
    break if c == map
    # (0...h).each do |y|
    #     (0...w).each do |x|
    #         print c[[x, y]] || '.'
    #     end
    #     puts
    # end
    map = c
end
```