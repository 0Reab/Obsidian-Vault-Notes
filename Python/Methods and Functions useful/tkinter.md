```python
import tkinter as tk  
  
calculation = ""  
  
def add_to_calculation(symbol):  
    global calculation  
    calculation += str(symbol)  
    text_result.delete(1.0, "end")  
    text_result.insert(1.0, calculation)  
  
def evaluate_calculation():  
    global calculation  
    try:  
        result = str(eval(calculation))  
        text_result.delete(1.0, "end")  
        text_result.insert(1.0, result)  
    except:  
        clear_field()  
        text_result.insert(1.0, "Error")  
  
def clear_field():  
    global calculation  
    calculation = ""  
    text_result.delete(1.0, "end")  
  
root = tk.Tk()  
root.geometry("300x275")  
  
text_result = tk.Text(root, height=2, width=16, font=("Arial", 24))  
text_result.grid(columnspan=5)  
  
# Define width and font that never change  
w = 5  
f = ("Arial", 14)  
  
def cmd(num):  
    return lambda: add_to_calculation(num)  
  
  
btn_1 = tk.Button(root, text="1", command=cmd(1), width=w, font=f)  
btn_1.grid(row=2, column=1)  
btn_2 = tk.Button(root, text="2", command=cmd(2), width=w, font=f)  
btn_2.grid(row=2, column=2)  
btn_3 = tk.Button(root, text="3", command=cmd(3), width=w, font=f)  
btn_3.grid(row=2, column=3)  
btn_4 = tk.Button(root, text="4", command=cmd(3), width=w, font=f)  
btn_4.grid(row=3, column=1)  
btn_5 = tk.Button(root, text="5", command=cmd(4), width=w, font=f)  
btn_5.grid(row=3, column=2)  
btn_6 = tk.Button(root, text="6", command=cmd(5), width=w, font=f)  
btn_6.grid(row=3, column=3)  
btn_7 = tk.Button(root, text="7", command=cmd(6), width=w, font=f)  
btn_7.grid(row=4, column=1)  
btn_8 = tk.Button(root, text="8", command=cmd(7), width=w, font=f)  
btn_8.grid(row=4, column=2)  
btn_9 = tk.Button(root, text="9", command=cmd(8), width=w, font=f)  
btn_9.grid(row=4, column=3)  
btn_0 = tk.Button(root, text="0", command=cmd(9), width=w, font=f)  
btn_0.grid(row=5, column=2)  
  
btn_plus = tk.Button(root, text="+", command=cmd("+"), width=w, font=f)  
btn_plus.grid(row=2, column=4)  
btn_minus = tk.Button(root, text="-", command=cmd("-"), width=w, font=f)  
btn_minus.grid(row=3, column=4)  
btn_mul = tk.Button(root, text="*", command=cmd("*"), width=w, font=f)  
btn_mul.grid(row=4, column=4)  
btn_div = tk.Button(root, text="/", command=cmd("/"), width=w, font=f)  
btn_div.grid(row=5, column=4)  
  
btn_par_opn = tk.Button(root, text="(", command=cmd("("), width=w, font=f)  
btn_par_opn.grid(row=5, column=1)  
btn_par_clos = tk.Button(root, text=")", command=cmd(")"), width=w, font=f)  
btn_par_clos.grid(row=5, column=3)  
  
btn_clr = tk.Button(root, text="clr", command=clear_field, width=11, font=f)  
btn_clr.grid(row=6, column=1, columnspan=2)  
  
btn_eq = tk.Button(root, text="=", command=evaluate_calculation, width=11, font=f)  
btn_eq.grid(row=6, column=3, columnspan=2)  
  
root.mainloop()
```