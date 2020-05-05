# Games
# PictoGames
## Description
PictoGames is a Unity made software inspired by [PictogramRoom](http://www.pictogramas.org/proom/loggined.do;jsessionid=7AA50E0020F6E27762B157DD7A9B7924) and focused on users with ASD.
Its main goal is to improve their motor, social and emotional skills.
Based on atusim researches, the application uses very simple designs and game dynamics so the users can feel comfortable and enjoy the interaction without any distraction.

PictoGames also uses XBOX ONE Kinect sensor technology to make possible an AR experience.
The game structure is very simple and can be recreated with new technologies.

Any recommendation is well received.

## Requirements
- Windows 7/8/10
- XBOX ONE Kinect sensor
- XBOX ONE Kinect sensor adapter for PC
- [Kinect for Windows Runtime 2.0](https://www.microsoft.com/en-us/download/details.aspx?id=44559)

## Installation
1. Download __PictoGamesInstaller.exe__
2. Follow the installation steps.
3. Run PictoGames directly from your start menu.

## Usage
Main menu shows three mini games. 
Each game menu has its own instructions in a piece of paper, a button to start to game and another button to return to the main menu.

### Lado a lado
in this game you have to touch the colored squares with the same color hand.
Be very carefull with your moves and position. Every time you touch a square with the wrong hand you loose a life. Try to get ten points to win.
If you reach zero lifes you lose.

### Manos arriba
In the next game you gotta keep moving from side to side on the screen trying to reach the green rectangles with your hands and head, every green rectangle will give you one point.
If you touch a red rectangle you lose a life.
Don't worry too much, once you have touched a rectangle you can freely move without loosing a life.
The max score you reach is ten points.
If you touch three red rectangles you lose.

### Recuerda el patrón
In the last game is very important to pay attention.
Remember the pattern of numbers in the right order. In six movements with your right hand you must recreate the pattern shown on the screen. If you did it right you'll win.
If you forget a move or one of the numbers you touched is wrong you lose.

## Documentation
If you want to edit or remake this application you can find the source code in the official website.
Here you'll find some helpful information that ease the development process by teaching how to use Kinect with the Unity editor.

### Recommendations
1. Once you have downloaded the source code, ensure to put the unzipped folder in a path less than 256 characters. Windows allows to use paths with a max of 256 characters.
2. Unity version: 2018.4.8
3. DO NOT touch anything out of the *Juego2019 folder*.

### Coding
Open Unity Hub and add the unzipped folder as a new project (2018.4.8), and open it.

Go to *Juego2019/Scenes* and open the scene with the name *EscenaPrincipal*.
This Scene is the base where you can use the default Kinect body shape with the PC camera.

Every user is detected by the Kinect sensor as a GameObject with the name __Body:*Incremental number*__ at the *KinectView/Scripts/BodySource.sc* script.

To find a player you can search the generated GameObject and assign it to a new GameObject
```c#
GameObject player = null;
void Update(){
  if(player == null){
    player = gameObject.Find("Body:1");
  }
}
```
If you need to find an specific body part their object names are attached to the found player body.
Here you can find the two hands and head.
```c#
if (player != null)
{
  playerHandLeft = player.transform.Find("HandLeft");
  playerHandRight = player.transform.Find("HandRight");
  playerHead = player.transform.Find("Head");
}
```
This is the dictionary for the body parts:

```c#
private Dictionary<Kinect.JointType, Kinect.JointType> _BoneMap = new Dictionary<Kinect.JointType, Kinect.JointType>()
{
  { Kinect.JointType.FootLeft, Kinect.JointType.AnkleLeft },
  { Kinect.JointType.AnkleLeft, Kinect.JointType.KneeLeft },
  { Kinect.JointType.KneeLeft, Kinect.JointType.HipLeft },
  { Kinect.JointType.HipLeft, Kinect.JointType.SpineBase },
  
  { Kinect.JointType.FootRight, Kinect.JointType.AnkleRight },
  { Kinect.JointType.AnkleRight, Kinect.JointType.KneeRight },
  { Kinect.JointType.KneeRight, Kinect.JointType.HipRight },
  { Kinect.JointType.HipRight, Kinect.JointType.SpineBase },
  
  { Kinect.JointType.HandTipLeft, Kinect.JointType.HandLeft },
  { Kinect.JointType.ThumbLeft, Kinect.JointType.HandLeft },
  { Kinect.JointType.HandLeft, Kinect.JointType.WristLeft },
  { Kinect.JointType.WristLeft, Kinect.JointType.ElbowLeft },
  { Kinect.JointType.ElbowLeft, Kinect.JointType.ShoulderLeft },
  { Kinect.JointType.ShoulderLeft, Kinect.JointType.SpineShoulder },
  
  { Kinect.JointType.HandTipRight, Kinect.JointType.HandRight },
  { Kinect.JointType.ThumbRight, Kinect.JointType.HandRight },
  { Kinect.JointType.HandRight, Kinect.JointType.WristRight },
  { Kinect.JointType.WristRight, Kinect.JointType.ElbowRight },
  { Kinect.JointType.ElbowRight, Kinect.JointType.ShoulderRight },
  { Kinect.JointType.ShoulderRight, Kinect.JointType.SpineShoulder },
  
  { Kinect.JointType.SpineBase, Kinect.JointType.SpineMid },
  { Kinect.JointType.SpineMid, Kinect.JointType.SpineShoulder },
  { Kinect.JointType.SpineShoulder, Kinect.JointType.Neck },
  { Kinect.JointType.Neck, Kinect.JointType.Head },
};
```
This information is available at the *KinectView/Scripts/BodySource.sc* script, but try not to change anything in this script unless you think it is necessary.

Body parts are very small. If you want the user to interact with virtual objects with an specific body part we recommend you to instantiate a GameObject in that body part.
Example:
```c#
public GameObject hat;
GameObject headObject = null;

void Update(){
 if (headObject == null && playerHead != null){
  headObject = Instantiate(hat, new Vector3(playerHead.transform.position.x, playerHead.transform.position.y, playerHead.transform.position.z), Quaternion.identity);
  headObject.name = "objCabeza"; //name the object
 }
}
```
Now you can use this object with collisions, triggers, etc.
Add each script you create using the Kinect sensor to the canvas in the scene and ensure to keep it active.

If you have any doubt, please let us know.

## License
Copyright (c) 2019 IAGames

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
