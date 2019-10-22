# a1
expenses
import openpyxl, os, tkinter
import pandas as pd
from tkinter import *


screen = tkinter.Tk()
screen.geometry("550x400")
screen.title("Expenses")


def calc_enter():
    filename = 'C:/Users/dummy/OneDrive/Desktop/1/Work/Expenses.xlsx'
    df = pd.read_excel(filename)
    total = str(round(df['Price'].sum(), 2))
    main.ent6.set(total + '$')


def view_info():
    filename = 'C:/Users/dummy/OneDrive/Desktop/1/Work/Expenses.xlsx'
    df = pd.read_excel(filename)
    print(df)


def file_path():
    path = 'C:/Users/dummy/OneDrive/Desktop/1/Work/Expenses.xlsx'
    path = os.path.realpath(path)
    os.startfile(path)
    exit()


class New:
    def __init__(self):
        self.prof_name = StringVar(screen)
        self.word = str(self.prof_name)
        self.wb_name = 'C:/Users/dummy/OneDrive/Desktop/1/Work/Expenses.xlsx'
        self.wb = openpyxl.load_workbook(self.wb_name)
        self.page = self.wb.active

    def sheet_window(self):
        self.window = Toplevel(screen)
        l1 = Label(self.window, text='New Profile')
        l1.pack(side='top')
        e1 = Entry(self.window, textvar=self.prof_name)
        e1.pack()
        b1 = Button(self.window, text='ENTER', bd=5, command=self.new_sheet)
        b1.pack()

    def new_sheet(self):
        self.wb.create_sheet(self.word)
        self.append_to()

    def append_to(self):
        filename = 'C:/Users/dummy/OneDrive/Desktop/1/Work/Expenses.xlsx'
        cats = {'Store', 'Item', 'Price', 'Payment_Method', 'Date_Purchased'}
        default_df = pd.DataFrame(data=cats)
        writer = pd.ExcelWriter(filename, engine='openpyxl')
        default_df.to_excel(writer, sheet_name=self.word)
        writer.save()
        self.window.destroy()
        self.wb.save(self.wb_name)


exc = New()


class Main_Window:
    def __init__(self, master):
        self.master = master
        self.pw = StringVar(screen)
        self.bloop = '1234'
        self.L1 = Label(master, text='eTracker', font=('arial', 40, 'bold')).pack(side='top')
        self.L2 = Label(master, text='Store:', font=('arial', 15)).place(x=140, y=85)
        self.L3 = Label(master, text='Item:', font=('arial', 15)).place(x=149, y=135)
        self.L4 = Label(master, text='Price:', font=('arial', 15)).place(x=143, y=185)
        self.L5 = Label(master, text='Payment_Method:', font=('arial', 15)).place(x=34, y=235)
        self.L6 = Label(master, text='Date_Purchased:', font=('arial', 15)).place(x=42, y=285)
        self.L7 = Label(master, text='Total', font=('arial', 25)).place(x=232, y=327)
        self.ent1 = StringVar(master)
        self.ent2 = StringVar(master)
        self.ent3 = DoubleVar(master)
        self.ent4 = StringVar(master)
        self.ent5 = StringVar(master)
        self.ent6 = StringVar(master)
        self.E1 = Entry(master, textvar=self.ent1, bd=5).place(x=210, y=86)
        self.E2 = Entry(master, textvar=self.ent2, bd=5).place(x=210, y=136)
        self.E3 = Entry(master, textvar=self.ent3, bd=5).place(x=210, y=186)
        self.E4 = Entry(master, textvar=self.ent4, bd=5).place(x=210, y=236)
        self.E5 = Entry(master, textvar=self.ent5, bd=5).place(x=210, y=286)
        self.E6 = Entry(master, textvar=self.ent6, bd=5, font=('arial', 15)).pack(side='bottom')
        self.M1 = Menu(master)
        self.profM1 = Menu(self.M1, tearoff=0)
        self.profM1.add_command(label='NEW', command=exc.sheet_window)
        self.profM1.add_command(label='LOAD')
        self.M1.add_cascade(label='PROFILES', menu=self.profM1)
        self.profM1.add_separator()
        self.profM1.add_command(label='EXIT', command=screen.quit)
        self.editM1 = Menu(self.M1, tearoff=0)
        self.editM1.add_command(label='SHOW', command=view_info)
        self.editM1.add_command(label='VIEW', command=file_path)
        self.M1.add_cascade(label='EDIT', menu=self.editM1)

    def clear(self):
        c1 = self.E1
        c1.delete(0, END)
        c2 = self.E2
        c2.delete(0, END)
        c3 = self.E3
        c3.delete(0, END)
        c4 = self.E4
        c4.self(0, END)
        c5 = self.E5
        c5.delete(0, END)


main = Main_Window(screen)


class Enter_Info:
    def __init__(self):
        self.wb = openpyxl.load_workbook(exc.wb_name)
        self.page = self.wb.active
        self.B1 = Button(screen, text='Enter Information', command=self.enter).place(x=400, y=131)
        self.B2 = Button(screen, text='Calculate Total', command=calc_enter).place(x=400, y=171)
        self.B3 = Button(screen, text='Clear', command=main.clear).place(x=400, y=91)

    def enter(self):
        inform = [main.ent1.get(), main.ent2.get(), main.ent3.get(), main.ent4.get(), main.ent5.get()]
        self.page.append(inform)
        self.wb.save(exc.wb_name)


info = Enter_Info()


screen.config(menu=main.M1)
screen.mainloop()
