# MaJyang
This is a project for fun, which includes support tool(s) for the online Mahjong game 雀魂/MahjongSoul. 
Currently supported feature(s):
- Automatically detect and classify your hand. Then, calculate the 向听数 / 向聴数 / number of required tiles to a ready hand + 有効牌 / 有效牌 / necessary tiles. With these suggestions, it can help you to learn how to play Mahjong in a efficient way.
- *In the codes, tiles are also called "PAIs".

# Other python packages you might need
- pyWinhook
- pythoncom
- win32api -> these three packages are necessary for taking screenshot; currently not support MacOS yet, you might have to modify the code and use different packages; see description later
- pillow -> general python image processing package
- numpy -> well known package for matrix operation
- opencv -> well known package of computer vision
- pandas -> well known package for xlsx / csv processing; used for data labelling
- torch
- torchvision -> well known machine learning framework
