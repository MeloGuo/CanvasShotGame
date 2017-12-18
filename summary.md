# 问题和解决方法总结
在完成项目的过程中遇到很多问题，在此记录一下问题的解决过程和思路。

#### 调用plane.move()的位置
如果使用事件绑定函数直接调用plane.move就会一卡一卡，如果使用requestAnimationFrame就没有这个问题。
所以应该在requestAnimationFrame中调用move，而事件绑定函数只负责判断恩下的按键是什么。
而发射子弹我们需要的就是摁下一次按钮发射一个子弹，所以应该在事件绑定函数中调用.
   
#### 重新绑定BUG
在初始的项目结构中，GAME.init方法中有一步this.bindEvent();操作。这个操作在单关时不会有影响，但是在进入下一关的时候，会重复绑定事件。
然后敌人和飞机的速度都会变快（原因没搞清楚）。所以要把this.bindEvent手动绑定到window.onload函数，这样每次重新开始游戏或者进入下一关的
时候就不会导致重复的事件绑定。

#### 敌人碰撞边框检测
在敌人碰撞边框的检测中，我一开始思路出现了错误。在循环上原始代码是这样：
  ```javascript
  for (var i = 0; i < monster.row; i++) {
    for (var j = 0; j < monster.column; j++) {
      if (monster.life[i][j]) {
        first = j;
        break;
      }
    }
  }  
  ```
这里出现的思维问题是，我们要得到的first变量中的是最左边的敌人的位置，但是在这个循环中出现的问题是：先循环了每一行的敌人。
而我们需要先循环的是每一列的敌人，如果这一列有一个活着的，那么就要以他为first。所以应该是如下循环：
  ```javascript
  for (var i = 0; i < monster.column; i++) {
    for (var j = 0; j < monster.row; j++) {
      if (monster.life[j][i]) {
        first = i;
        break;
      }
    }
  }
  ```