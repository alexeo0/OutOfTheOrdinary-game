import tkinter as tk
import random

def brosok():
    global d
    d = random.randint(1, 6)
    kubik.config(text=str(d))

    kub.config(state='disabled')
    x10.config(state='active')
    plus.config(state='active')

def f(bool):
    global points, d
    if check() == True:
        kubik.config(text='-')
        if bool == True:
            points += d*10
        else:
            points += d
        point.config(text=f'Счёт: {points}')

        kub.config(state='active')
        x10.config(state='disabled')
        plus.config(state='disabled')
    if check() == False:
        kub.config(state='disabled')
        x10.config(state='disabled')
        plus.config(state='disabled')
        
        if points == 100:
            win.pack()
        elif points > 100:
            lose.pack()

def check():
    global points
    if points < 100:
        return True
    else:
        return False

points, d = 0, 0

t = tk.Tk()
t.title('Игра с разрядами чисел')
t.geometry('240x320')
t.configure(bg='white')

kubik = tk.Label(t, text='-', font=('Comicsans', 32))
kubik.pack(pady=5)

point = tk.Label(t, text=f'Счёт: 0', font=('Comicsans', 20))
point.pack(pady=5)

kub = tk.Button(t, text='Бросить кубик', command=brosok, state='active')
kub.pack(pady=20)

x10 = tk.Button(t, text='Умножить на 10', command=lambda: f(True), state='disabled')
x10.pack(pady=2)

plus = tk.Button(t, text='Оставить', command=lambda: f(False), state='disabled')
plus.pack(pady=2)

win, lose = tk.Label(t, text='Вы выиграли', font=('Comicsans', 25)), tk.Label(t, text='Вы проиграли', font=('Comicsans', 25))


t.mainloop()
