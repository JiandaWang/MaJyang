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
  
Also, since the GitHub has a upload limit of 25mb using browser, the pre-trained model is zipped. Please un-zip it first under Model\\.
  
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

# Train the model by yourself
In case you are not satisfied with the performance of the pre-trained model, you can also do it by yourself from the very begin.  

## Raw screenshot
To train the model, you have to build your database first. And the start point of that would be collecting raw screenshots from the game.  
You can run the notebook "Screenshot", and it will monitor you keyboard input just like the notebook "Application".  
Everytime you press "C", one screenshot will be saved into Database\Raw_Screenshots. The file name would looks like YYYYMMDD_HHMMSS.  
Please refer other details in the chapter "Plug and Play".

## Separate into tiles
After getting the raw screenshots, you have to separate them into single tiles for further training. For that, you can run the notebook "Separation".
It will read all screenshots from Database\Raw_Screenshots, and put output tiles into Database\EgoHand_Separated_PAIs.  
The file name would looks like YYYYMMDD_HHMMSS_00, YYYYMMDD_HHMMSS_01... and so on.  
Remember you have to do the same adption for the resolution which was mentioned in the chapter "Plug and Play".

## Labelling
Database needs not only the data, but also the label. Run the notebook "LabelSupport", it will automatically collect all the file names in Database\EgoHand_Separated_PAIs and generate a "Label.xlsx". (CSV file is usually the solution, but you can use google docs to edit xlsx for free. So you don't have to buy office.)  
  
Now you have to manually label those pictures. You should create a new column called "Label" for it. The rules are simple:  
- 万/萬子/Characters: 1m..9m
- 条/索子/Bamboos: 1s..9s
- 饼/筒子/Circles: 1p..9p
- 东/東/East: E
- 南/South: S
- 西/West: W
- 北/North: N
- 发/発/Green dragon: G
- 中/Red dragon: R
- 白/White dragon: Wh
  
After manually labelled the pictures, you should run the notebook again. It will extend the file names to full path and automatically generate a new column called "Label_ID", corresponding to the labels. These values are the target value later would be used in training.

## Normalization
Data should be normalized for training. Please run the notebook "NormalizationSupport". Since the notebook import in jupyter works in the way, that the source notebook would be excuted completely once, so if we import the output mean and standard deviation from "NormalizationSupport" into other notebooks, the whole calculation would be done again. Therefor, please manually copy those values into notebook "Training" and "Application" later.

## Training
You can run the notebook "Training". Please remember to update those values from "NormalizationSupport".  
The pre-implemented code will try to take 80% data from your database (everytime different part) for training, and do it 5 times.  
In this notebook you have lot of options, like you can use pre-trained ResNet18 as start point, or not trained ResNet18, or the pre-trained model in Model\\. You can even use other well known models, or build it by yourself. Feel free to change the code.  
At the end, there is one line commented out, you can use it to save the model you have trained.

## Play
You can run now the notebook "Application" to see the performance. Besides the other hints given before, please remember to update the values from "NormalizationSupport". Also, please update the ***g_PreTrainedModelPath_str*** to your model.
