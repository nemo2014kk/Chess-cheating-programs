# 中國象棋輔助工具（其實就是傳説中的“物理外挂”啦！😄）
每次和別人玩象棋都會輸，但現在不會了。因爲接下來我將設計一款輔助工具來協助我下象棋，如果這款工具設計成功，從此人腦變AI，棋力大漲。
這是我設計的一款中國象棋或其他棋類的輔助工具，目前只有硬件，還沒有代碼。需要一個精通編程的老師跟我共同完成這個項目。
它分爲Arduino與RC522 rfid之間的通訊與控制、Arduino與上位機之間的通訊與控制
（軟件基於開源的“象棋巫師”https://github.com/liangqi/xqwizard/tree/master/XQWIZARD）所有的代碼都是開源的，
所有的目的只是爲了軟硬件技術的學習、交流，提升電子技術水平。如果您有興趣一起做這個項目，歡迎您與我聯係！期待您的來信！Email:nemo2014kk@gmail.com

This is an auxiliary tool for Chinese chess or other chess games designed by me.
Currently, there is only hardware and no code.
I need a teacher who is proficient in programming to work with me on this project. 
It is divided into communication and control between Arduino and RC522 rfid, Arduino and host computer
(the software is based on the open source "chess wizard" https://github.com/liangqi/xqwizard/tree/master/XQWIZARD) 
all the codes are Open source, all the purpose is only for learning and communication of software and hardware technology, 
and improving the level of electronic technology.If you are interested in doing this project together,
you are welcome to contact me! We look forward to hearing from you!
Email:nemo2014kk@gmail.com
![9811 jpg_wh300](https://user-images.githubusercontent.com/121267030/210174168-2f919964-b1f2-4e87-8a10-246df3e59c49.jpg)




整個系統分為兩大部分

一、arduino 與RC522 RFID之間的通訊。參考https://github.com/esprfid/esp-rfid

1、整機只用了一塊RC522讀卡器，但是我們需要讀取90個棋盤位置，於是我拆分了90個接收線圈，分佈到棋盤的90個位置，所以每次描整個棋盤就需要讀取90次。

2、使用Arduino控制CD4067（16路模擬開關 https://www.ti.com/lit/ds/symlink/cd4067b.pdf）
依次接通，切換不同的接收線圈，來依次讀取每個線圈上面的ID值。

共有6组CD4067, Arduino的指令在每个CD4067的使能EN脚（第15脚）进行切换不同的组。然后分別控制CD4067的ABCD 4个脚位值，就能够实现遍历90个线圈，这样也就得到了棋盘上每个位置的棋子ID。如果棋子位置發生了改變，通過不停的掃描比對，就能夠得出ID位置的差異，這樣也就得知了現實中棋盤上的棋子的路綫、位置。

二、Arduino與上位機象棋軟體之間的通訊

1、Arduino與RFID掃描到的ID數據差異，結果要傳輸到上位機，也就是告訴象棋軟體中的“人”該走如何走棋，相當於用Arduino替代了象棋軟體中的鼠標點擊走棋的動作。

2、象棋軟體我選用“象棋巫師”https://github.com/liangqi/xqwizard/tree/master/XQWIZARD，這是開源的軟體。
   它以“象眼“https://github.com/liangqi/xqwizard/tree/master/ELEEYE為引擎，
   有AI的强大算力，棋力蠻厲害的。




工作流程：

1、因中國象棋有規定紅棋先行，所以，電腦象棋軟體設置為：電腦AI執黑棋。

2、這樣，開局的時候AI不會有動作，等待象棋軟體中的“人”（紅棋）走棋。

3、象棋軟體中的“人”（紅棋）=現實中我的對手（紅棋）。

4、現實中“我”執黑棋，讓對方執紅棋先行，只要對方紅棋一動，就會被Arduino偵測到，反饋給電腦，象棋軟體中的“人”就會根據Arduino偵測到的現實中棋盤上紅棋ID的變化，走一步棋。 

5、然後象棋軟體AI（黑棋）自動走一步棋，並語音朗讀此步，現實中的“我”通過藍牙耳機得知，於是“我”按AI朗讀的着法走一步相同的棋（黑棋）。

6、然後又輪到現實中我的對手（紅棋）走棋，只要紅旗一動，又會被Arduino偵測到，反饋給電腦，象棋軟體中的“人”又會根據Arduino偵測到的紅棋ID變化，走一步棋。AI（黑棋）自動走一步棋 並語音朗讀……

7、如此循環往復，直至棋局結束。


我們的代碼任務：

1、Arduino與RC522 RFID 結合，控制CD4067，實現掃描棋盤的功能，得出90個ID數據。

2、再次掃描整個ID，進行ID數據比對，找出差異，推算出是哪個棋子位置發生了變化。

3、Arduino與上位機象棋軟體之間的通訊與控制，根據Arduino推算出的棋子變化，告訴象棋軟體中的 ”人“該如何走棋。



Ps:這是“象棋巫師”源碼https://github.com/liangqi/xqwizard/tree/master/XQWIZARD，
自述文件https://github.com/liangqi/xqwizard/blob/master/XQWIZARD/DOC/FAQ.TXT

據説：
"2. 象棋巫师是用什么语言开发的？
象棋巫师是用 Visual Basic 6.0 开发的。 
VB6是 Visual Studio 98 的一个组件，安装 Visual Studio 98 后，"XQWIZARD.VBP"即被关联为VB6的工程文件。双击VBP文件即可进入VB6的集成开发环境，执行VBP文件右键菜单中的Make，即可编译成象棋巫师执行程序(XQWIZARD.EXE)了。"


![未命名](https://user-images.githubusercontent.com/121267030/210295665-ec726d86-d9c9-4acc-b0ad-974b0b8a1399.png)




The whole system is divided into two parts

1. Communication between Arduino and RC522 RFID.  reference example https://github.com/esprfid/esp-rfid

(1). The whole machine only uses one RC522 card reader, but we need to read 90 chessboard positions, so I split 90 receiving coils and distributed them to 90 positions on the chessboard, so every time we scan the entire chessboard, we need to read Take 90 times.

(2). Use Arduino to control CD4067 (16-channel analog switch https://www.ti.com/lit/ds/symlink/cd4067b.pdf)
Turn on in turn, switch different receiving coils, and read the ID value on each coil in turn.

There are 6 groups of CD4067, and the Arduino command switches between different groups at the EN pin (pin 15) of each CD4067. Then control the ABCD 4 pin values of CD4067 respectively, so that 90 coils can be traversed, so that the chess piece ID of each position on the chessboard can be obtained. If the position of the chess piece changes, the difference in the ID position can be obtained through continuous scanning and comparison, so that the route and position of the chess pieces on the real chessboard can be known.

2. Communication between Arduino and PC chess software

(1). The ID data scanned by Arduino and RFID are different, and the results are transmitted to the host computer, which is to tell the "person" in the chess software how to move, which is equivalent to replacing the mouse click and move chess in the chess software with Arduino action.


(2)As the chess software, I choose "Chess Wizard" https://github.com/liangqi/xqwizard/tree/master/XQWIZARD, which is open source software.
    It uses "Elephant Eye" https://github.com/liangqi/xqwizard/tree/master/ELEEYE as the engine,
    With the powerful computing power of AI, the chess skills are quite powerful.


work process:

1. Because Chinese chess stipulates that the red chess goes first, the computer chess software is set as follows: the computer AI executes the black chess.

2. In this way, the AI will not make any moves at the beginning of the game, waiting for the "person" (red chess) in the chess software to move.

3. The "person" (red chess) in the chess software = my opponent (red chess) in reality.

4. In reality, "I" hold the black piece and let the opponent hold the red piece first. As long as the opponent's red piece moves, it will be detected by the Arduino and fed back to the computer. The "person" in the chess software will detect it according to the Arduino In reality, the red chess ID on the chessboard changes, and a move is made.

5. Then the chess software AI (Black Chess) automatically takes a move and reads the move by voice. The real "I" knows it through the Bluetooth headset, so "I" moves the same move according to the AI reading (Black Chess) chess).

6. Then it’s my opponent’s (red chess) turn to move in reality. As long as the red flag moves, it will be detected by Arduino and fed back to the computer. The chess ID changes, and a move is made. AI (Black Chess) automatically moves a move and reads it aloud...

7, go round and round like this, until the end of chess game.


Our code task:

1. Combine Arduino with RC522 RFID, control CD4067, realize the function of scanning the chessboard, and obtain 90 ID data.

2. Scan the entire ID again, compare the ID data, find out the difference, and calculate which piece's position has changed.

3. The communication and control between Arduino and the host computer chess software tells the "person" in the chess software how to play chess according to the changes of chess pieces calculated by Arduino.


Ps: This is the source code of "Chess Wizard" https://github.com/liangqi/xqwizard/tree/master/XQWIZARD,
Readme file https://github.com/liangqi/xqwizard/blob/master/XQWIZARD/DOC/FAQ.TXT

It is said that:
"2. What language is Chess Wizard developed in?
Chess Wizard was developed with Visual Basic 6.0.
VB6 is a component of Visual Studio 98. After installing Visual Studio 98, "XQWIZARD.VBP" is associated as the project file of VB6. Double-click the VBP file to enter the VB6 integrated development environment, execute Make in the right-click menu of the VBP file, and then compile it into a chess wizard execution program (XQWIZARD.EXE). "

![未命名 - 複製](https://user-images.githubusercontent.com/121267030/210297018-8c29b374-a268-4320-8277-02be1608c262.png)

