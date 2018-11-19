# Ruby/GraphViz
[![All Contributors](https://img.shields.io/badge/all_contributors-32-orange.svg?style=flat-square)](#contributors)

[<img src="https://secure.travis-ci.org/glejeune/Ruby-Graphviz.svg"
/>](https://travis-ci.org/glejeune/Ruby-Graphviz) [<img
src="https://badge.fury.io/rb/ruby-graphviz.svg" alt="Gem Version"
/>](https://rubygems.org/gems/ruby-graphviz)

Copyright (C) 2004-2018 Gregoire Lejeune

* Doc : http://rdoc.info/projects/glejeune/Ruby-Graphviz
* Sources : https://github.com/glejeune/Ruby-Graphviz
* On Rubygems: https://rubygems.org/gems/ruby-graphviz

## DESCRIPTION

Interface to the GraphViz graphing tool

## TODO

* New FamilyTree


## SYNOPSIS

A basic example

```ruby
require 'graphviz'

# Create a new graph
g = GraphViz.new( :G, :type => :digraph )

# Create two nodes
hello = g.add_nodes( "Hello" )
world = g.add_nodes( "World" )

# Create an edge between the two nodes
g.add_edges( hello, world )

# Generate output image
g.output( :png => "hello_world.png" )
```

The same but with a block

```ruby
require 'graphviz'

GraphViz::new( :G, :type => :digraph ) { |g|
  g.world( :label => "World" ) << g.hello( :label => "Hello" )
}.output( :png => "hello_world.png" )
```

Or with the DSL

```ruby
    require 'graphviz/dsl'
    digraph :G do
      world[:label => "World"] << hello[:label => "Hello"]

      output :png => "hello_world.png"
    end
```

Create a graph from a file 

```ruby
    require 'graphviz'

    # In this example, hello.dot is :
    #   digraph G {Hello->World;}

    GraphViz.parse( "hello.dot", :path => "/usr/local/bin" ) { |g|
      g.get_node("Hello") { |n|
        n[:label] = "Bonjour"
      }
      g.get_node("World") { |n|
        n[:label] = "Le Monde"
      }
    }.output(:png => "sample.png")
```

[GraphML](http://graphml.graphdrawing.org/) support

```ruby
    require 'graphviz/graphml'

    g = GraphViz::GraphML.new( "graphml/cluster.graphml" )
    g.graph.output( :path => "/usr/local/bin", :png => "#{$0}.png" )
```

## TOOLS

Ruby/GraphViz also includes :

* `ruby2gv`, a simple tool that allows you to create a dependency graph from a ruby script. Example : http://drp.ly/dShaZ

```ruby
ruby2gv -Tpng -oruby2gv.png ruby2gv
```

* `gem2gv`, a tool that allows you to create a dependency graph between gems. Example : http://drp.ly/dSj9Y

```ruby
gem2gv -Tpng -oruby-graphviz.png ruby-graphviz
```

* `dot2ruby`, a tool that allows you to create a ruby script from a graphviz script

```ruby
$ cat hello.dot
digraph G {Hello->World;}

$ dot2ruby hello.dot
# This code was generated by dot2ruby.g

require 'rubygems'
require 'graphviz'
graph_g = GraphViz.digraph( "G" ) { |graph_g|
  graph_g[:bb] = '0,0,70,108'
  node_hello = graph_g.add_nodes( "Hello", :height => '0.5', :label => '\N', :pos => '35,90', :width => '0.88889' )
  graph_g.add_edges( "Hello", "World", :pos => 'e,35,36.413 35,71.831 35,64.131 35,54.974 35,46.417' )
  node_world = graph_g.add_nodes( "World", :height => '0.5', :label => '\N', :pos => '35,18', :width => '0.97222' )
}
puts graph_g.output( :canon => String )
```

* `git2gv`, a tool that allows you to show your git commits

* `xml2gv`, a tool that allows you to show a xml file as graph.


## GRAPH THEORY

```ruby
require 'graphviz'
require 'graphviz/theory'

g = GraphViz.new( :G ) { ... }

t = GraphViz::Theory.new( g )

puts "Adjancy matrix : "
puts t.adjancy_matrix

puts "Symmetric ? #{t.symmetric?}"

puts "Incidence matrix :"
puts t.incidence_matrix

g.each_node do |name, node|
  puts "Degree of node `#{name}' = #{t.degree(node)}"
end

puts "Laplacian matrix :"
puts t.laplacian_matrix

puts "Dijkstra between a and f"
r = t.moore_dijkstra(g.a, g.f)
if r.nil?
  puts "No way !"
else
  print "\tPath : "; p r[:path]
  puts "\tDistance : #{r[:distance]}"
end

print "Ranges : "
rr = t.range
p rr
puts "Your graph contains circuits" if rr.include?(nil)

puts "Critical path : "
rrr = t.critical_path
print "\tPath "; p rrr[:path]
puts "\tDistance : #{rrr[:distance]}"
```

## INSTALLATION

```
sudo gem install ruby-graphviz
```

You also need to install [GraphViz](http://www.graphviz.org) 

On **Windows** you also need to install win32-open3. This is not an absolute
requirement.

## LICENCES

### Ruby/GraphViz 

Ruby/GraphViz is freely distributable according to the terms of the GNU
General Public License (see the file 'COPYING').

This program is distributed without any warranty. See the file 'COPYING' for
details.

GNU General Public Licence: https://en.wikipedia.org/wiki/Gpl

### nothugly.xsl

By Vidar Hokstad and Ryan Shea; Contributions by Jonas Tingborn, Earl
Cummings, Michael Kennedy (Graphviz 2.20.2 compatibility, bug fixes, testing,
lots of gradients)

Copyright (c) 2009 Vidar Hokstad

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

MIT license: https://en.wikipedia.org/wiki/MIT_License

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars0.githubusercontent.com/u/139988?v=4" width="100px;"/><br /><sub><b>Dave Burt</b></sub>](https://github.com/dburt)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=dburt "Code") | [<img src="https://avatars2.githubusercontent.com/u/146361?v=4" width="100px;"/><br /><sub><b>obruening</b></sub>](https://github.com/obruening)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=obruening "Code") | [<img src="https://avatars0.githubusercontent.com/u/49928?v=4" width="100px;"/><br /><sub><b>reactive</b></sub>](https://github.com/reactive)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=reactive "Code") | [<img src="https://avatars2.githubusercontent.com/u/21093?v=4" width="100px;"/><br /><sub><b>axgle</b></sub>](https://github.com/axgle)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=axgle "Code") | [<img src="https://avatars1.githubusercontent.com/u/22006?v=4" width="100px;"/><br /><sub><b>Chip Malice</b></sub>](https://github.com/hipe)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=hipe "Code") | [<img src="https://avatars3.githubusercontent.com/u/101456?v=4" width="100px;"/><br /><sub><b>Stefan Stüben</b></sub>](https://github.com/MSNexploder)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=MSNexploder "Code") | [<img src="https://avatars0.githubusercontent.com/u/5942?v=4" width="100px;"/><br /><sub><b>Nigel Thorne</b></sub>](http://www.nigelthorne.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=NigelThorne "Code") |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [<img src="https://avatars1.githubusercontent.com/u/78237?v=4" width="100px;"/><br /><sub><b>Rolf Timmermans</b></sub>](https://github.com/rolftimmermans)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=rolftimmermans "Code") | [<img src="https://avatars3.githubusercontent.com/u/359500?v=4" width="100px;"/><br /><sub><b>Jonas Elfström</b></sub>](http://alicebobandmallory.com/)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=jonelf "Code") | [<img src="https://avatars1.githubusercontent.com/u/143470?v=4" width="100px;"/><br /><sub><b>oupo</b></sub>](https://github.com/oupo)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=oupo "Code") | [<img src="https://avatars1.githubusercontent.com/u/15168?v=4" width="100px;"/><br /><sub><b>Gregoire Lejeune</b></sub>](http://lejeun.es)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=glejeune "Code") [🎨](#design-glejeune "Design") [📖](https://github.com/glejeune/Ruby-Graphviz/commits?author=glejeune "Documentation") | [<img src="https://avatars0.githubusercontent.com/u/591567?v=4" width="100px;"/><br /><sub><b>Markus Hauck</b></sub>](https://github.com/markus1189)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=markus1189 "Code") | [<img src="https://avatars2.githubusercontent.com/u/308027?v=4" width="100px;"/><br /><sub><b>Khalil Fazal</b></sub>](https://github.com/khalilfazal)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=khalilfazal "Code") | [<img src="https://avatars2.githubusercontent.com/u/1180335?v=4" width="100px;"/><br /><sub><b>Kenichi Kamiya</b></sub>](https://github.com/kachick)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=kachick "Code") |
| [<img src="https://avatars0.githubusercontent.com/u/1448453?v=4" width="100px;"/><br /><sub><b>Neven Has</b></sub>](https://github.com/nevenh)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=nevenh "Code") | [<img src="https://avatars1.githubusercontent.com/u/1844746?v=4" width="100px;"/><br /><sub><b>Andrew</b></sub>](https://Andrew.Kvalhe.im)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=AndrewKvalheim "Code") | [<img src="https://avatars3.githubusercontent.com/u/245206?v=4" width="100px;"/><br /><sub><b>Daniel Zollinger</b></sub>](https://github.com/dznz)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=dznz "Code") | [<img src="https://avatars0.githubusercontent.com/u/531168?v=4" width="100px;"/><br /><sub><b>Guilherme Simoes</b></sub>](https://guilhermesimoes.github.io)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=guilhermesimoes "Code") | [<img src="https://avatars0.githubusercontent.com/u/2205407?v=4" width="100px;"/><br /><sub><b>Oleg Orlov</b></sub>](https://github.com/OrelSokolov)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=OrelSokolov "Code") | [<img src="https://avatars3.githubusercontent.com/u/222582?v=4" width="100px;"/><br /><sub><b>Gabe Kopley</b></sub>](https://www.instagram.com/stewiecutedog/)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=gkop "Code") | [<img src="https://avatars0.githubusercontent.com/u/174509?v=4" width="100px;"/><br /><sub><b>Jake Goulding</b></sub>](http://jakegoulding.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=shepmaster "Code") |
| [<img src="https://avatars0.githubusercontent.com/u/898442?v=4" width="100px;"/><br /><sub><b>hirochachacha</b></sub>](https://github.com/hirochachacha)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=hirochachacha "Code") | [<img src="https://avatars2.githubusercontent.com/u/125620?v=4" width="100px;"/><br /><sub><b>ronen barzel</b></sub>](http://www.ronenbarzel.org)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=ronen "Code") | [<img src="https://avatars1.githubusercontent.com/u/72027?v=4" width="100px;"/><br /><sub><b>Jamison Dance</b></sub>](https://jamison.dance)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=jergason "Code") | [<img src="https://avatars3.githubusercontent.com/u/7495?v=4" width="100px;"/><br /><sub><b>Joseph Anthony Pasquale Holsten</b></sub>](http://josephholsten.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=josephholsten "Code") | [<img src="https://avatars0.githubusercontent.com/u/12527?v=4" width="100px;"/><br /><sub><b>Miguel Cabrera</b></sub>](http://mfcabrera.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=mfcabrera "Code") | [<img src="https://avatars0.githubusercontent.com/u/529516?v=4" width="100px;"/><br /><sub><b>Mike Fiedler</b></sub>](http://www.miketheman.net)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=miketheman "Code") | [<img src="https://avatars2.githubusercontent.com/u/338814?v=4" width="100px;"/><br /><sub><b>Nathan Long</b></sub>](http://nathanmlong.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=nathanl "Code") |
| [<img src="https://avatars0.githubusercontent.com/u/211?v=4" width="100px;"/><br /><sub><b>Olle Jonsson</b></sub>](http://ollehost.dk/)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=olleolleolle "Code") | [<img src="https://avatars2.githubusercontent.com/u/12671?v=4" width="100px;"/><br /><sub><b>Postmodern</b></sub>](http://postmodern.github.com/)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=postmodern "Code") | [<img src="https://avatars0.githubusercontent.com/u/652130?v=4" width="100px;"/><br /><sub><b>Robert Reiz</b></sub>](https://www.versioneye.com)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=reiz "Code") | [<img src="https://avatars2.githubusercontent.com/u/1724196?v=4" width="100px;"/><br /><sub><b>Göran Bodenschatz</b></sub>](http://46halbe.de)<br />[💻](https://github.com/glejeune/Ruby-Graphviz/commits?author=coding46 "Code") |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!