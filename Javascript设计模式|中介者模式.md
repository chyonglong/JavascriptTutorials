#### Javascript设计模式|中介者模式
定义：

中介者模式的作用就是解除对象和对象之间的紧耦合关系。增加一个中介者对象后，所有的相关对象都通过中介者来通信，而不是相互引用，所以当一个对象发生改变时，只需要通知中介者对象就可以了。让之前的网状关系变成了一对多的关系

现实生活中的中介者
显示生活中有很多中介者的例子，例如

机场指挥塔
中介者也称为调停者，如果没有机场的指挥塔存在，每一架飞机都要和方圆100公里内的所有飞机通信，才能确定航线和飞行状况，这是难以想象的。显示中的情况就是，每架飞机起飞或者降落的时候和指挥塔通信，指挥塔知道每架飞机的情况和跑道的情况，他可以合理的做出决策
博彩公司
在世界杯期间买足球彩票，如果没有博彩公司作为中介，上千万的人一起计算赔率和输赢是不可能的事情啊，有了博彩公司作为中介，每个人只需要和博彩公司发生关联就可以了，输赢都去找他们
中介者例子–泡泡堂游戏
我们先看最简单的例子，游戏只支持两个玩家同时进行对战，定义一个构造函数和3个方法win、close和die

function Player(name) {
    this.name = name;
    this.enemy = null;
}

Player.prototype.win = function() {
    console.log(this.name + 'won');
}

Player.prototype.lose = function() {
    console.log(this.name + 'lost');
}

Play.prototype.die = function() {
    this.lose();
    this.enemy.win();
}

var player1 = new Player('player1');
var palyer2 = new Player('player2');

player1.enemy = player2;
palyer2.enemy = player1;
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
当palyer1被炸死的时候，只需要调用player1.die()就可以了，但是实际上没这么简单，玩过泡泡堂的都知道，游戏里面之多可以有8个玩家，并且分为红蓝两队进行游戏。

我们定义一个数组players来保存所有玩家，在创建玩家之后，循环players来给每个玩家设置队友和敌人。

function Player(name, teamColor) {
    this.partners = [];
    this.enemies = [];
    this.state = 'live';
    this.name = name;
    this.teamColor = teamColor;
}
1
2
3
4
5
6
7
失败或者成功后还是很简单，直接输出就可以了

Player.prototype.win = function() {
    console.log('winner: ' + this.name);
}
Player.prototype.lose = function() {
    console.log('loser: ' + this.name);
}
1
2
3
4
5
6
玩家死亡的方法需要复杂点，每个玩家死亡的时候，都遍历一遍其他队友的情况，如果队友都死了，则这句游戏失败，同时敌人队伍的所有玩家都取得胜利

Player.prototype.die = function() {
    var all_dead = true;
    this.state = 'dead';
    for (var i = 0, partner; partner = this.partner[i++]) {
        if (partner.state !== 'dead') {
            all_dead = false;
            break;
        }
    }
    if (all_dead === true) {
        this.lose();
        for(var i = 0, partner; partner= this.partner[i++]) {
            partner.lose();
        }
        for(var i = 0, enemy; enemy = this.enemies[i++]) {
            enemy.win();
        }
    }
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
然后我们用一个工厂来创建玩家

var playerFactory = function(name, teamColor) {
    var newPlayer = new Player(name, teamColor);
    for (var i = 0, palyer; player = player[i++]) {
        if (player.teamColor === newPlayer.teamColor) {
            player.partners.push(newPlayer);
            newPlayer.partners.push(player);
        } else {
            player.enemies.push(newPlayer);
            newPlayer.enemies.push(player);
        }
    }
    players.push(newPlayer);
    return newPlayer;
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
创建玩家：

var player1 = playerFactory('qqq', 'red');
var player2 = playerFactory('www', 'red');
var player3 = playerFactory('eee', 'red');
var palyer4 = playerFactory('rrr', 'red');

var player5 = playerFactory('aaa', 'blue');
var player6 = playerFactory('sss', 'blue');
var player7 = playerFactory('ddd', 'blue');
var player8 = playerFactory('fff', 'blue');
1
2
3
4
5
6
7
8
9
红队都阵亡:

player1.die();
player2.die();
player3.die();
player4.die();
1
2
3
4
我们考虑下，上面代码还有什么问题？每个玩家和其他玩家都是紧密耦合在一起的，在这段代码中，每个玩家都有两个属性this.partners和this.enemies，用来保存其他玩家对象的引用，每次有角色死亡或者移动就要显示的遍历通知所有的其他对象。
这里只有8个，想想如果是80、800，或者玩家可以叛变，这就不是循环能解决的问题了。

接下来我们用中介者模式来改造泡泡堂游戏

首先定义Player构造函数和player对象的原型方法，在palyer对象的这些原型方法中，不再负责具体的逻辑，而是把操作转交给中介者对象，我们命名为playerDirector

function Player(name, teamColor) {
    this.name = name;
    this.teamColor = teamColor;
    this.state = 'alive';
}

Player.prototype.win = function() {
    console.log(this.name + ' won ');
}

Player.prototype.lose = function() {
    console.log(this.name + ' lost ');
}

Player.prototype.die = function() {
    this.state = 'dead';
    playerDirector.receiveMessage('playerDead', this);
}

Player.prototype.remove = function() {
    playerDirector.receiveMessage('removePlayer', this);
}

Player.prototype.changeTeam = function(color) {
    playerDirector.receiveMessage('changeTeam', this, color);
}

var playFactory = function(name, teamColor) {
    var newPlayer = new Player(name, teamColor);
    playerDirector.receiveMessage('addPlayer', newPlayer);
    return newPlayer;
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
接下来就是我们如何来实现中介者playerDirector对象，一般来说有两种方式：

发布订阅模式，playerDirector是订阅者，palyer是发布者，一旦player发生改变，就推送消息给playerDirector，然后中介者处理消息后反馈给其他player
在playerDirector中开发一些接受消息的接口，各个player可以调用这个接口来给playerDirector发送消息，player传递参数给playerDirector，参数的目的是让playerDirector识别发送者。
这两种方式实现本质上没有区别
var playerDirector = (function() {
    var players = {},
        operations = {};
    operations.addPlayer = function(player) {
        var teamColor = player.teamColor;
        players[teamColor] = players[teamColor] || [];
        players[teamColor].push(player);
    };
    operations.removePlayer = function(player) {
        var teamColor = player.teamColor,
            teamPlayers = players[teamColor] || [];
            for (var i = teamPlayers.length - 1; i >=0; i--) {
                if (teamPlayers[i] === player) {
                    teamPlayers.splice(i, 1);
                }
            }
    };
    operations.changeTeam = function(player, newTeamColor) {
        operations.removePlayer(player);
        player.teamColor = newTeamColor;
        opeations.addPlayer(player);
    };
    operations.playerDead = function(player) {
        var teamColor = player.teamColor,
            teamPlayers = palyers[teamColor];
            var all_dead = true;
            for (var i = 0, player; player = teamPlayers[i++]; ){
                if (player.state !== 'dead') {
                    all_dead = false;
                    break;  
                }
            }

            if (all_dead === true) {
                for (var i = 0, palyer; player = teamPlayers[i++]; ){
                    player.lose();
                }
                for (var color in players) {
                    if (color !== teamColor) {
                        var teamPlayers = players[color];
                        for (var i = 0, player; player = teamPlayers[i++]; ) {
                            player.win();
                        }
                    }
                }
            }

    };
    var recieveMessage = function() {
        var message = Array.prototype.shift.call(arguments);
        operations[message].apply(this, arguments);
    };
    return {
        receiveMessage: recieveMessage
    };
})();
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
我们可以看到，除了中介者本身，没有一个玩家是知道其他任何玩家的存在的，玩家和玩家之间耦合关系基本没有，中介者还可以根据游戏的需要增加更多的功能

小结
中介者模式是最少知识原则的一种实现，是指一个对象应该尽可以能少的了解另外的对象。如果对象的耦合性太高，一个对象的改变难免会影响到其他的对象。
中介者最大的缺点在于系统中会增加一个中介者对象，因为对象之间的交互的复杂性，转移成为了中介者对象的复杂性，是的中介者对象可能很庞大，难以维护。
一般来说，如果对象之间的复杂耦合导致调用和维护出现困难，而且这些耦合度随着项目的变化呈指数增加曲线的时候，我们就可以考虑用中介者模式来重构代码。
————————————————
版权声明：本文为CSDN博主「lihangxiaoji」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lihangxiaoji/article/details/80179073


---
> 桥智科技：科技赋能梦想！专注广州、深圳和惠州小程序定制开发、APP 应用定制开发、网站开发、区块链钱包开发！