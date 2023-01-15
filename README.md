# DataDriven-Application

import tkinter as tk
import requests
from PIL import Image, ImageTk
from io import BytesIO
from PIL import Image

# Api url
url_api = "https://www.thecocktaildb.com/api/json/v1/1"


root = tk.Tk()
root.geometry("1024x768")

buttons_font = ("Lobster", 15, "bold")
font_H = ("Lobster", 22, "bold")
TEXT = ("Lobster", 18, "italic")
para = ("Lobster", 12, "italic")
name= ("Lobster", 18, "italic")

# Titile for the project
root.title("The Cocktail Finder")

# Create homepage function 
def homepg():
 
    for widget in frame_main.winfo_children():
        widget.destroy()
    
    displaydrinks.pack(side= "right")

    for widget in displaydrinks.winfo_children():
        widget.destroy()

    heading_main = tk.Label(
        frame_main,
        text="The Cocktail Planet\n________",   
        font=font_H,
        bg="#a32638",
        fg="#f8b22d",
    )
    heading_main.place(x=490, y=100)
    
    homepg_display = tk.Label(
        frame_main,
        text="Welcome to the Cocktail Planet\n here you will find different\n types of drinks and drinks ",
        fg="#f8b22d",
        bg="#a32638",
        font= TEXT, 
        justify="center",
        wraplength=760,
    )
    homepg_display.place(x=480, y=400)


# Function for Ordinary drinks page  where the list is going to be displayed
def Ordinary_Drinks(cocktail_type):

    for widget in frame_main.winfo_children():
        widget.destroy()
    displaydrinks.place(x=10, y=235)
    heading = tk.Label(
        frame_main,
        text="The Cocktail palace\nMenu for Ordinary Drinks\n________",
        font=font_H,
        bg="#a32638",
        fg="#f8b22d",
    )
    heading.pack(side="top", anchor="center", pady=30)

    # Take the data of drinks from the api url
    output = requests.get(f"{url_api}/filter.php?a={cocktail_type}")
    drinks = output.json()["drinks"]
    design_grid = tk.Frame(frame_main, bg="#a32638")
    #  Display the drinks
    for i, cocktail in enumerate(drinks, start=0):
        if i == 20:
            break
        num_id = cocktail["idDrink"]
        display_name = cocktail["strDrink"]
        btn_border = tk.Frame(
            design_grid,
            highlightbackground="#f8b22d",
            highlightthickness=1,
            bd=1,
        )
        button = tk.Button(
            btn_border,
            text=display_name,
            font=TEXT,
            bg="#a32638",
            fg="#f8b22d",
            bd=0,
            width=25,
            justify="right",
            command=lambda id=num_id: selctions(id),
        )
        button.pack()
        btn_border.grid(row=i // 2, column=i % 2, padx=5, pady=5)
    design_grid.pack(side="right", padx=10)

    for widget in displaydrinks.winfo_children():
        widget.destroy()


# Function for Cocktails page  where the list is going to be displayed
def Cocktails(cocktail_type):

    for widget in frame_main.winfo_children():
        widget.destroy()
    displaydrinks.place(x=10, y=235)
    heading = tk.Label(
        frame_main,
        text="The Cocktail palace\nHere is the Menu for Cocktails\n________",
        font=font_H,
        bg="#a32638",
        fg="#f8b22d",
    )
    heading.pack(side="top", anchor="center", pady=30)

    # Take data from the api url
    output = requests.get(f"{url_api}/filter.php?a={cocktail_type}")
    drinks = output.json()["drinks"]
    design_grid = tk.Frame(frame_main)
    # Display drinks
    for i, cocktail in enumerate(drinks, start=0):
        if i == 20:
            break
        num_id = cocktail["idDrink"]
        display_name = cocktail["strDrink"]
        btn_border = tk.Frame(
            design_grid,
            highlightbackground="#f8b22d",
            highlightthickness=1,
            bd=1,
        )
        button = tk.Button(
            btn_border,
            text=display_name,
            font=TEXT,
            bg="#a32638",
            fg="#f8b22d",
            bd=0,
            width=25,
            justify="right",
            command=lambda id=num_id: selctions(id),
        )
        button.pack()
        btn_border.grid(row=i // 2, column=i % 2, padx=5, pady=5)
    design_grid.pack(side="right", padx=10)

    for widget in displaydrinks.winfo_children():
        widget.destroy()


def selctions(id):

    # Remove previous drinks 
    for widget in displaydrinks.winfo_children():
        widget.destroy()

    # Take the cocktail data from the API
    output = requests.get(f"{url_api}/lookup.php?i={id}")
    cocktail = output.json()["drinks"][0]

    # Extract the cocktail details
    drink_name = cocktail["strDrink"]
    img_url = cocktail["strDrinkThumb"]

    #Image downlad and display in the page
    dataimg = requests.get(img_url).content
    img = Image.open(BytesIO(dataimg))
    img = img.resize((300, 300), Image.ANTIALIAS)
    img = ImageTk.PhotoImage(img)
    image_label = tk.Label(displaydrinks, image=img, bd=5, bg="#a32638")
    image_label.image = img 
    image_label.pack(side="top", anchor="center")

    # Cocktail name display
    displayname = tk.Label(
        displaydrinks,
        text=f"___\n\n{drink_name}\n___",
        font=TEXT,
        fg="#f8b22d",
        bg="#a32638",
    )
    displayname.pack(side="top")
    # Id display
    id_label = tk.Label(
        displaydrinks,
        text=f"Drink ID: {id}",
        font=TEXT,
        fg="#f8b22d",
        bg="#a32638",
    )
    id_label.pack(side="top")


# Exit the project 
def close():
    root.destroy()


# The function is used to erase the data of previous selected pages
def remove_data():
    for frame in frame_main.winfo_children():
        frame.destroy()

# create the frame on the right side 
slection_frame = tk.Frame(root, bg="#f8b22d")

# Create the logo.
logo = tk.Label(
    slection_frame,
    text="Tԋҽ \n  Cσƈƙƚαιʅ \n Pʅαɳҽƚ ",
    fg="#a32638",
    bg="#fcf75e",
    bd=0,
    
    height=5,
    width=20,
    font=name,
    justify="center",
)
logo.place(x=0, y=0)

# home button
home_btn = tk.Button(
    slection_frame,
    text="Home",
    font=buttons_font,
    fg="#f8b22d",
    bg="#a32638",
    bd=0,
    command= homepg)

home_btn.place(x=10, y=150)


# main button
ordinary_btn = tk.Button(
    slection_frame,
    text="Ordinary Drinks",
    font=buttons_font,
    fg="#f8b22d",
    bg="#a32638",
    bd=0,
    command= lambda: Ordinary_Drinks("Alcoholic"))

ordinary_btn.place(x=10, y=240)


# info button
cocktail_btn = tk.Button(
    slection_frame,
    text="Cocktails",
    font=buttons_font,
    fg="#f8b22d",
    bg="#a32638",
    bd=0,
    command= lambda: Cocktails("Non_Alcoholic")
    )

cocktail_btn.place(x=10, y=300)

close_btn = tk.Button(
    slection_frame,
    text="CLOSE",
    font=buttons_font,
    fg="#f8b22d",
    bg="#a32638",
    bd=5,
    command=exit,
)
close_btn.place(x=50, y=760)
slection_frame.pack(side=tk.RIGHT)
slection_frame.pack_propagate(False)
slection_frame.configure(width=220, height=900) 


# frame to display the info when the buttons are clicked
frame_main = tk.Frame(root, bg="#a32638")

# frame for the drinks display.
displaydrinks = tk.Frame(root, bg="#f8b22d")
displaydrinks.place(x=10, y=235)

frame_main.pack()  
frame_main.configure(width=1500, height=900)
frame_main.propagate(0)

lb = tk.Label(
    frame_main,
    text="- Welcome To The Cocktail Planet ",
    font=font_H,
    bg="#a32638",
    fg="#f8b22d",
)
lb.place(x=400, y=30)


root.attributes("-fullscreen", True)
root.overrideredirect(False)
root.mainloop()
