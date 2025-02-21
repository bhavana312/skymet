This is a **Weather Alert System** built using **Python's Tkinter** for the GUI, **requests** for fetching weather data from OpenWeatherMap API, and **plyer** for sending desktop notifications.

---

### **Code Breakdown:**

#### **1. Importing Required Libraries**
```python
import time
from tkinter import *
from tkinter import messagebox as mb
import requests
from plyer import notification
```
- `tkinter`: Used for GUI creation.
- `requests`: Fetches weather data from OpenWeatherMap API.
- `plyer.notification`: Displays weather notifications on the system.
- `time`: Adds delay between notifications.

---

#### **2. Creating the Tkinter Window**
```python
wn = Tk()
wn.title("Weather Alert System")
wn.geometry('800x500')
wn.config(bg='#E8F1F5')
```
- Initializes the main window (`wn`) with a title, fixed size (`800x500` pixels), and a light blue background (`#E8F1F5`).

---

#### **3. Defining Styling Constants**
```python
PRIMARY_COLOR = "#1B4965"  
SECONDARY_COLOR = "#5FA8D3"  
BG_COLOR = "#E8F1F5"  
FONT_TITLE = ("Helvetica", 24, "bold")
FONT_LABEL = ("Helvetica", 14)
FONT_BUTTON = ("Helvetica", 12, "bold")
```
- Defines colors and fonts for a modern UI.

---

#### **4. Creating Main Frame**
```python
main_frame = Frame(wn, bg=BG_COLOR, padx=40, pady=40)
main_frame.place(relx=0.5, rely=0.5, anchor=CENTER)
```
- The `main_frame` holds all UI elements and is centered in the window.

---

#### **5. Adding Title Label**
```python
title_label = Label(
    main_frame,
    text="Weather Alert System",
    font=FONT_TITLE,
    fg=PRIMARY_COLOR,
    bg=BG_COLOR,
    pady=20
)
title_label.pack()
```
- Displays the app title in large, bold text.

---

#### **6. Input Field for Location**
```python
input_frame = Frame(main_frame, bg=BG_COLOR, pady=30)
input_frame.pack()

Label(
    input_frame,
    text='Enter Location:',
    font=FONT_LABEL,
    bg=BG_COLOR,
    fg=PRIMARY_COLOR
).pack(side=LEFT, padx=10)

place = StringVar(wn)
place_entry = Entry(
    input_frame,
    width=30,
    textvariable=place,
    font=FONT_LABEL,
    relief="solid",
    bd=1
)
place_entry.pack(side=LEFT, padx=10)
```
- `place_entry` allows users to enter a city name.
- `StringVar` stores the input dynamically.

---

#### **7. Fetching Weather Data and Sending Notification**
```python
def getNotification():
    cityName = place.get()
    baseUrl = "http://api.openweathermap.org/data/2.5/weather"
    try:
        url = baseUrl + "?q=" + cityName + "&appid=d850f7f52bf19300a9eb4b0aa6b80f0d"
        response = requests.get(url)
        x = response.json()
        y = x["main"]
        
        temp = y["temp"] - 273
        pres = y["pressure"]
        hum = y["humidity"]
        z = x["weather"]
        weather_desc = z[0]["description"]

        info = f"""Weather in {cityName}:
Temperature: {temp:.1f}°C
Pressure: {pres} hPa
Humidity: {hum}%
Conditions: {weather_desc.title()}"""

        notification.notify(
            title="WEATHER REPORT",
            message=info,
            timeout=2
        )
        time.sleep(7)
    
    except Exception as e:
        mb.showerror('Error', 'Could not fetch weather data. Please check the city name and try again.')
```
##### **How It Works:**
1. Fetches user input (`cityName`).
2. Constructs API request URL (`OpenWeatherMap API`).
3. Sends request and retrieves JSON data.
4. Extracts:
   - Temperature (converted from Kelvin to Celsius).
   - Pressure.
   - Humidity.
   - Weather conditions (e.g., cloudy, rainy).
5. Formats the data into a user-friendly string.
6. Displays a system notification with the weather report.
7. If an error occurs (e.g., invalid city name), an error message is shown.

---

#### **8. Creating a Button to Trigger Weather Alert**
```python
button_frame = Frame(main_frame, bg=BG_COLOR, pady=20)
button_frame.pack()

notify_btn = Button(
    button_frame,
    text='Get Weather Report',
    font=FONT_BUTTON,
    fg='white',
    bg=SECONDARY_COLOR,
    padx=20,
    pady=10,
    relief="flat",
    command=getNotification,
    cursor="hand2"
)
notify_btn.pack()
```
- Clicking this button triggers `getNotification()`.

---

#### **9. Adding Hover Effect on Button**
```python
def on_enter(e):
    notify_btn['bg'] = PRIMARY_COLOR

def on_leave(e):
    notify_btn['bg'] = SECONDARY_COLOR

notify_btn.bind("<Enter>", on_enter)
notify_btn.bind("<Leave>", on_leave)
```
- When the mouse hovers over the button, it changes color.

---

#### **10. Running the Tkinter Main Loop**
```python
wn.mainloop()
```
- Keeps the application in running mode
