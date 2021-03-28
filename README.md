# MaJyang
This is a project for fun, which includes support tool(s) for the online Mahjong game 雀魂/MahjongSoul.  
This tool is written and will be excuted in form of jupyter notebook.  
Currently supported feature(s):
- Automatically detect and classify your hand. Then, calculate the 向听数 / 向聴数 / number of required tiles to a ready hand + 有効牌 / 有效牌 / necessary tiles. With these suggestions, it can help you to learn how to play Mahjong in a efficient way. (*In the codes, tiles are also called "PAIs".)

# Other python packages you might need
- jupyter notebook
- import_ipynb -> to import already defined values / functions in other notebooks
- pyWinhook
- pythoncom
- win32api -> these three packages are necessary for taking screenshot; currently not support MacOS yet, you might have to modify the code and use different packages; see description later
- pillow -> general python image processing package
- numpy -> well known package for matrix operation
- opencv -> well known package of computer vision
- pandas -> well known package for xlsx / csv processing; used for data labelling
- torch
- torchvision -> well known machine learning framework

# “Plug and Play”
If you have Windows (10) as operation system and you screen resolution is 1920x1080, you should be able to directly use the prepared notebook "Application".  
Before first time using the tool, you might have to manually set both font paths ***g_FontPath_str*** and ***g_PAIFontPath_str***. Especially to display the tiles, you need one ttf font which support utf-8 Mahjong tiles. Please refer the comment in the code.
