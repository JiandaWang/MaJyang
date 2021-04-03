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
If your screen resolution is 1920x1080, you should be able to directly use the prepared notebook "Application".  
(Note: as mentioned before, if you are not using Windows, you have to use other packages to monitor your keyboard input. Please modify the ***onKeyboardEvent*** and ***main*** function.)  

Before first time using the tool, you might have to manually set both font paths ***g_FontPath_str*** and ***g_PAIFontPath_str***. Especially to display the tiles, you need one ttf font which support utf-8 Mahjong tiles. Please refer the comment in the code.
  
After you opened the notebook, just click "Cell->Run All" to run the tool. After the excution, the tool will start to monitor your keyboard input.  
There are two valid inputs:
- "Esc": stop the tool.
- "C": the tool will take a screenshot and try to separate single tiles from your hand. After that, those tiles will be classified with a pre-trained model. At the end, the tool will calculate your necessary tiles based on a DFS algorithm. The result will be displayed in a new picture.  
  
Note: the tool is expecting certain form of the screenshot. 
- It will only work after you draw a tile and before you play any tile from your hand.
- Currently, it only supports when your hand is 门清 / メンゼンチン / all concealed.
- Please use default single color background in game for stability. 
- Please ensure your hand in the screenshot is not covered by any animation effect or your mouse cursor.

# Adjust the resolution
If your resolution is different, you still can use the pre-trained model, but with some small adaption.  
Note: some parameters are imported from the notebook "Separation". You will need to do changes there.  

- ***g_EgoHandCropLeftUpperX_int, g_EgoHandCropLeftUpperY_int, g_EgoHandCropRightLowerX_int, g_EgoHandCropRightLowerY_int***:  
These parameters define the upper left and lower right corner of the rectangle including all tiles in your hand. Please refer the pillow documention for the coordinate definition. ***Unit: pixel***
- ***g_EgoHandCropMinPaiArea_int***:  
Minimal area for one tile. This thershold is used to filter out possible small contour of "noise" from the screenshot. ***Unit: pixel^2***
- ***g_EgoHandGaussKSize_int***:  
As the screenshot is taken directly from the game, usually no Gaussian filter is necessary. But since the tiles are displayed very closed to each other, we have to blur the screenshot a little bit so that the contours can be found correctly. ***Unit: pixel***
- ***g_AvgW_int, g_AvgH_int***:  
Average width and height of one tile. ***Unit: pixel***
