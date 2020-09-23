<div align="center">

## Make Transparent Forms by a Forms Picture


</div>

### Description

This will make a Form Transparent according to a Pixel of it's Picture. Be patient with me as I am learning how to code in C++. This code was ported from a project in VB by Chris Yates. However VB is horribly slow so I thought I'd make a Dll in C++ that did the same. Here's the result
 
### More Info
 
0

In the VB runtime you can call the Dll but the Function will do 1 of 2 things either no Transparencies will show or the whole form will disappear. However, after you compile it works fine.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Shawn Elliott](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/shawn-elliott.md)
**Level**          |Beginner
**User Rating**    |4.5 (18 globes from 4 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Controls/ Forms/ Dialogs/ Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/controls-forms-dialogs-menus__3-3.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/shawn-elliott-make-transparent-forms-by-a-forms-picture__3-541/archive/master.zip)





### Source Code

```
/*This code is derived from Visual Basic code developed by :
Chris Yates
cyates@neo.rr.com
This has been ported by :
Shawn Elliott
selliott@speedgate.net
This code comes with no warranty whatsoever and is FREE for distribution without Royalty fees of any kind
*/
#include "stdafx.h"
#include "Windows.h"
#include "Windef.h"
int __stdcall MakeTransparent(HWND,int,int,int,int,int);
BOOL APIENTRY DllMain( HANDLE hModule,
            DWORD ul_reason_for_call,
            LPVOID lpReserved
					 )
{
  return TRUE;
}
int __stdcall MakeTransparent(HWND WinHandle,int Red,int Blue, int Green,int Width,int Height)
{
/*Varaible Declarations*/
int X;
int Y;
HRGN CurRgn;
HRGN TempRgn;
bool set;
set = FALSE;
COLORREF Pixel,Transparent;
HDC hdc;
/*Assign our values*/
hdc = GetDC( WinHandle );
CurRgn = CreateRectRgn(0, 0, Width, Height);
Transparent = RGB(Red,Green,Blue);
X = 0;
Y = 0;
do
{
	do
	{
		Pixel = GetPixel(hdc,X,Y);
		if (Pixel == Transparent)
		{
				set = TRUE;
				/*This is a Pixel that we want to make transparent*/
				TempRgn = CreateRectRgn(X,Y,X + 1,Y + 1);			/*Create a Temporary Region with this Location*/
				CombineRgn(CurRgn, CurRgn, TempRgn, RGN_DIFF);		/*Combine the Temporary Region with the Created one*/
				DeleteObject(TempRgn);								/*Clean the Temporary Region from Memory*/
		}
	X = X + 1;
	if (X > Width) break;
	}while(FALSE == FALSE);
	X = 0;
	Y = Y + 1;
if (Y > Height) break;
}while(FALSE == FALSE);
/*Now we set the Region to the Calling Handle if there was any changes*/
if (set == TRUE)
{
	SetWindowRgn(WinHandle, CurRgn, TRUE);	/*Set the Region*/
}
ReleaseDC( WinHandle, hdc );
DeleteObject(CurRgn);					/*Clean up the Region we worked with*/
return 0;
} /* End Transparency */
```

