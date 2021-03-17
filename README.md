# Paint
 #a paint app using python
from tkinter import *
import tkinter.ttk as ttk
from tkinter import colorchooser
from tkinter import filedialog
from PIL import Image,ImageDraw,ImageGrab,ImageTk
import PIL
from tkinter import messagebox

root=Tk()
root.title("Program.com Paint Program")
root.geometry('800x800')

brush_color = "black"

def paint(e):
	#my_canvas.config(bg="black")

	#brush parameters 
	brush_width = "%0.0f" % float(my_slider.get())
	#brush_color = "green"
	#brush_type : BUTT,ROUND,PROJECTING
	brush_type2 = brush_type.get()

	#starting position
	x1=e.x-1
	y1=e.y-1

	#ending position
	x2=e.x+1
	y2=e.y+1

	#draw on canvas 
	my_canvas.create_line(x1,y1,x2,y2,fill=brush_color, width=brush_width, capstyle=brush_type2, smooth=True)
	
#change size of brush
def change_brush_size(thing):
	slider_label.config(text="%0.0f" % float(my_slider.get()))

#change brush color
def change_brush_color():
	global brush_color
	brush_color="black"
	brush_color=colorchooser.askcolor(color=brush_color)[1]

#change canvas color
def change_canvas_color():
	global bg_color
	bg_color="black"
	bg_color=colorchooser.askcolor(color=brush_color)[1]
	my_canvas.config(bg=bg_color)
	#color =label(root,text=brush_color)
	#color.pack(pady=20)

#clear screen
def clear_screen():
	my_canvas.delete(ALL)
	my_canvas.config(bg='white')

#save image
def save_as_png():
	result=filedialog.asksaveasfilename(initialdir="c:/paint/images",filetypes=(("png files","*.png"),("all files","*.*")))

	if result.endswith(".png"):
		pass
	else:
		result=result+".png"

	if result:
		tx=root.winfo_rootx()+my_canvas.winfo_x()
		ty=root.winfo_rooty()+my_canvas.winfo_y()
		tx1=tx+my_canvas.winfo_width()
		ty1=ty+my_canvas.winfo_height()
		ImageGrab.grab().crop((tx,ty,tx1,ty1)).save(result)

	#pop up success message box

	messagebox.showinfo("Image saved ","Your image has been saved ")

	result_label=Label(root,text=result)
	result_label.pack(pady=20)


#create your canvas
w=600
h=400

my_canvas = Canvas(root,width=w,height=h,bg="white")
my_canvas.pack(pady=20)

#my_canvas.create_line(0,100,300,100,fill="red")
#my_canvas.create_line(150,0,150,200,fill="red")

my_canvas.bind("<B1-Motion>",paint)

#brush options frame
brush_options_frame = Frame(root)
brush_options_frame.pack(pady=20)

#brush size
brush_size_frame = LabelFrame(brush_options_frame,text="brush size")
brush_size_frame.grid(row=0,column=0,padx=50)

#brush slider
my_slider = ttk.Scale(brush_size_frame,from_=1,to=100, command=change_brush_size,orient=VERTICAL, value=10)
my_slider.pack(pady=10,padx=10)

#brush slider label
slider_label = Label(brush_size_frame,text=my_slider.get())
slider_label.pack(pady=5)

#brush type
brush_type_frame = LabelFrame(brush_options_frame,text="brush type",height=400)
brush_type_frame.grid(row=0,column=1,padx=50)

brush_type = StringVar()
brush_type.set("round")

#create radio buttons for brush type
brush_type_radio1= Radiobutton(brush_type_frame, text="round", variable=brush_type, value="round")
brush_type_radio2= Radiobutton(brush_type_frame, text="slash", variable=brush_type, value="butt")
brush_type_radio3= Radiobutton(brush_type_frame, text="diamond", variable=brush_type, value="projecting")

brush_type_radio1.pack(anchor=W)
brush_type_radio2.pack(anchor=W)
brush_type_radio3.pack(anchor=W)

#change colors
change_colors_frame = LabelFrame(brush_options_frame, text="change colors")
change_colors_frame.grid(row=0, column=2)

#change brush color button
brush_color_button = Button(change_colors_frame, text="brush colors",command=change_brush_color)
brush_color_button.pack(pady=10, padx=10)

#change canvas background
canvas_color_button = Button(change_colors_frame, text="canvas color", command=change_canvas_color)
canvas_color_button.pack(pady=10 , padx=10)

#program options frame
options_frame = LabelFrame(brush_options_frame, text="Programs options")
options_frame.grid(row=0 , column=3,padx=50)

#clear screen button
clear_button =Button(options_frame, text="clear screen",command=clear_screen)
clear_button.pack(padx=10 , pady=10)

#save image
save_image_button =Button(options_frame, text="save to PNG", command=save_as_png)
save_image_button.pack(pady=10, padx=10)

root.mainloop()




