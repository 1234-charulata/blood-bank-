# blood-bank-
from tkinter import *
from PIL import Image,ImageTk
from scrolling_area import *
import tkinter.messagebox
from datetime import date
import time
import smtplib
import ttkcalendar
import tkSimpleDialog
import sqlite3,mysql.connector

root = Tk()
root.title("Blood Bank")
root.geometry('1366x768+0+0')
root.config(bg="powder blue")
root.resizable(0,1)
'''
class CalendarDialog(tkSimpleDialog.Dialog):
    def body(self,m):
        self.calendar = ttkcalendar.Calendar(m)
        self.calendar.pack()
    def apply(self):
        self.result = self.calendar.selection
'''
name   =  StringVar()
t_date   =  StringVar()
dob    =  StringVar()
email  =  StringVar()
address = StringVar()
contact = StringVar()
bloodgroup = StringVar()

def home():
    frame1 = Frame(root,width =1366,height=768)
    frame1.place(x=0,y=0)

    img1 = ImageTk.PhotoImage(Image.open("img1.jpg"))
    Label(frame1,image=img1,width=1366,height=768).place(x=0,y=0)

    l1 = Label(frame1,text="Welcome",font=('arial',30,'bold','italic'),height=1,width=15,fg='red',bg='black')
    l1.place(x=500,y=10)

    b1 = Button(frame1,text='ADD',font=('arial',12,'bold'),fg='blue',width=10,bd=4,command=lambda:add(frame1))
    b1.place(x=460,y=330)

    b2 = Button(frame1,text='SHOW',font=('arial',12,'bold'),fg='blue',width=10,bd=4,command=lambda:show(frame1))
    b2.place(x=780,y=330)

    b3 = Button(frame1,text="EXIT",font=('arial',12,'bold'),fg='red',width=10,bd=3,command=lambda:exit())
    b3.place(x=640,y=650)

    frame1.mainloop()


def exit():
    exit = tkinter.messagebox.askquestion(title='Blood Bank', message='You want to exit "Blood Bank"?')
    if exit=='yes':
        quit()
    else:
        pass

# textvariable is used for reset the entry and submit the entry in database
def add(frame1):
    frame1.destroy()
    frame2 = Frame(root,width=768,height=1366,bg='powder blue')
    frame2.pack()

    img2 = ImageTk.PhotoImage(Image.open("img2.jpg"))
    Label(frame2,image=img2,width = 1366,height=768).pack()

    l2 = Label(frame2,text='Name : ',font=('arial',15,'bold'),fg='red')
    l2.place(x=400,y=200)
    e2 = Entry(frame2,width=25,bd=4,font=('arial',13,'bold'),textvariable = name)
    e2.place(x=600,y=200)

    l3 = Label(frame2,text="Blood Group : ",font=('arial',15,'bold'),fg='red')
    l3.place(x=400,y=240)
    bloodgroup.set("A+")
    list = ["A+","B+","O+","A-","B-","O-","AB+","AB-"]
    e3 = OptionMenu(frame2,bloodgroup,*list)
    e3.config(width=31,bd=3,bg="steel blue")
    e3.place(x=600,y=240)

    l4 = Label(frame2,text='Date : ',font=('arial',15,'bold'),fg='red')
    l4.place(x=400,y=280)
    t_date = date.today().strftime("%d-%m-%Y")                                    # get current date
    e4 = Entry(frame2,width=25,bd=4,font=('arial',13,'bold'),textvariable=t_date)
    e4.insert(0,t_date)
    e4.place(x=600,y=280)

    l5 = Label(frame2, text='Date Of Birth : ', font=('arial', 15, 'bold'), fg='red')
    l5.place(x=400, y=320)
    #global selected_date
    #selected_date = StringVar()
    e5 = Entry(frame2, width=25, bd=4, font=('arial', 13, 'bold'),textvariable = dob)
    e5.place(x=600, y=320)
    #b = Button(frame2,text='choose dob',width=10,height=1,command=lambda:getdate(frame2))
    #b.place(x=800,y=329)

    l6 = Label(frame2, text='Email : ', font=('arial', 15, 'bold'), fg='red')
    l6.place(x=400, y=360)
    e6 = Entry(frame2, width=25, bd=4, font=('arial', 13, 'bold'),textvariable = email)
    e6.place(x=600, y=360)


    l7 = Label(frame2, text='Contact No. : ', font=('arial', 15, 'bold'), fg='red')
    l7.place(x=400, y=400)
    contact.set("+91 ")
    e7 = Entry(frame2, width=25, bd=4, font=('arial', 13, 'bold'),textvariable = contact)
    e7.place(x=600, y=400)

    l8 = Label(frame2, text='Address : ', font=('arial', 15, 'bold'), fg='red')
    l8.place(x=400, y=440)
    e8 = Entry(frame2, width=25, bd=4, font=('arial', 13, 'bold'),textvariable = address)
    e8.place(x=600, y=440)

    b4 = Button(text="Submit",font=('arial',10,'italic'),fg='green',bd=4,width=8,command=lambda:submit(e2,bloodgroup,e4,e5,e6,e7,e8))
    b4.place(x=590,y=530)

    b5 = Button(text="Back", font=('arial', 10, 'italic'), fg='green', bd=4, width=8, command=lambda:back2(frame2))
    b5.place(x=430, y=530)

    b6 = Button(text="Reset", font=('arial', 10, 'italic'), fg='green', bd=4,width=8,command=lambda:reset())
    b6.place(x=750, y=530)

    def reset():
        name.set("")
        bloodgroup.set("A+")
        email.set("")
        contact.set("+91 ")
        dob.set("")
        address.set("")

    def back2(frame2):
        frame2.destroy()
        return home()

    frame2.mainloop()


def show(frame1):
    check1 = IntVar()
    check2 = IntVar()
    check3 = IntVar()
    check4 = IntVar()
    check5 = IntVar()
    check6 = IntVar()
    check7 = IntVar()
    check8 = IntVar()

    frame1.destroy()
    frame3 = Frame(root,width=1366,height=768,bg='powder blue')
    frame3.pack()

    img3 = ImageTk.PhotoImage(Image.open("img3.jpg"))
    Label(frame3,image=img3,width=1366,height=768).pack()

    l9 = Label(frame3,text='Select Blood Group :-',font=('arial',15,'italic','bold'),width=20)
    l9.place(x=150,y=100)

    c1 = Checkbutton(frame3,text='A+',font=('arial',12,'bold'),fg='green',onvalue=1,offvalue=0,width=5,bd=3,variable=check1,padx=2)
    c1.place(x=150,y=150)

    c2 = Checkbutton(frame3,text='B+',font=('arial',12,'bold'),fg='green',width=5,bd=3,variable=check2,padx=2)
    c2.place(x=150,y=190)

    c3 = Checkbutton(frame3,text='O+',font=('arial',12,'bold'),fg='green',width=5,bd=3,variable=check3,padx=2)
    c3.place(x=150,y=230)

    c4 = Checkbutton(frame3,text='A-',font=('arial',12,'bold'),fg='green',width=5,bd=3,variable=check4,padx=0).place(x=150,y=270)

    c5 = Checkbutton(frame3,text='B-',font=('arial',12,'bold'),fg='green',width=5,bd=3,variable=check5,padx=0)
    c5.place(x=150,y=310)

    c6 = Checkbutton(frame3,text='O-',font=('arial',12,'bold'),fg='green',width=5,bd=3,variable=check6,padx=0)
    c6.place(x=150,y=350)

    c7 = Checkbutton(frame3, text='AB-', font=('arial', 12, 'bold'), fg='green', width=5, bd=3,variable=check7,padx=4)
    c7.place(x=150, y=390)

    c8 = Checkbutton(frame3, text='AB+', font=('arial', 12, 'bold'), fg='green', width=5, bd=3,variable=check8,padx=5)
    c8.place(x=150, y=430)

    b7 = Button(frame3,text='Back',font=('arial',10,'italic'),fg='green',width=5,bd=4,command=lambda:back3())
    b7.place(x=170,y=480)

    b8 = Button(frame3,text='Reset',font=('arial',10,'italic'),fg='green',width=5,bd=4,command=lambda:reset())
    b8.place(x=230,y=480)

    b9 = Button(frame3,text='Ok',font=('arial',10,'italic'),fg='green',width=5,bd=4,command=lambda:b_ok(frame3,root,check1,check2,check3,check4,check5,check6,check7,check8))
    b9.place(x=290,y=480)

    def reset():
        check1.set(0)
        check2.set(0)
        check3.set(0)
        check4.set(0)
        check5.set(0)
        check6.set(0)
        check7.set(0)
        check8.set(0)

    def back3():
        frame3.destroy()
        home()

    frame3.mainloop()


def b_ok(frame3,root,check1,check2,check3,check4,check5,check6,check7,check8):
    #frame3.destroy()
    frame4 = Frame(root,width=1366,height=768,bg='powder blue')
    frame4.place(x=0,y=0)

    img4 = ImageTk.PhotoImage(Image.open("img3.jpg"))
    Label(frame4,image=img4,width=1366,height=768).pack()

    scrolling_area = Scrolling_Area(frame4,height=300,width=1350)
    scrolling_area.place(x=0,y=0)

    table = Table(scrolling_area.innerframe,["Name","Blood_Group","Date","Date Of Birth",
                                             "Email","Contact No.","Address "],
                                             column_minwidths = [190,180,190,190,190,190,200])
    table.pack(expand=TRUE,fill=X)
    table.on_change_data(scrolling_area.update_viewport)

    receiver_email = []                               # create a empty list for receiver_email
    con = sqlite3.connect("Blood_Bank")

    def show_table(var):
        output = list(con.execute(f"select * from Blood_Bank_Data where Blood_Group = '{var}'"))
        con.commit()
       # print(output)
        data = []
        for row in output:
            column = []
            print(row)                                # print data on terminal
            data.append(column)
            for r in row:
                #print(r)
                column.append(r)
        table.set_data(data)

        _,_,_,_,Email,_,_ = zip(*output)     # Access Email from database using zip function and store in Email
        receiver_email.append(Email)
        print('\n Receiver_Email_Id\'s = ',receiver_email)


    if check1.get()==1:
        show_table('A+')
    elif check2.get()==1:
        show_table('B+')
    elif check3.get()==1:
        show_table('O+')
    elif check4.get()==1:
        show_table('A-')
    elif check5.get()==1:
        show_table('B-')
    elif check6.get()==1:
        show_table('O-')
    elif check7.get()==1:
        show_table('AB-')
    elif check8.get()==1:
        show_table('AB+')

    b10 = Button(frame4,text='Back',font=('arial',10,'italic'),fg='green',width=5,bd=4,command=lambda:back4())
    b10.place(x=300,y=400)

    b11 = Button(frame4, text='Send Email', font=('arial', 10, 'italic'), fg='green', width=10, bd=4,
                 command=lambda: send_mail(con,receiver_email))
    b11.place(x=600, y=400)

    def back4():
        frame4.destroy()

    def send_mail(con,receiver_email):
        try:
            con = smtplib.SMTP('smtp.gmail.com',587)
            con.ehlo()
            con.starttls()
            con.login('ishusingh9587@gmail.com','ishusingh')
            message = "Subject:Donate Your Blood \n\n Here is need of blood. If you are free, Donate your blood and save someone life."

            send_message = tkinter.messagebox.askquestion(title="Send Email",message="You want to send Email to all")
            if send_message == 'yes':
                con.sendmail('ishusingh9587@gmail.com', receiver_email, message)
                tkinter.messagebox.showinfo(title="Successful",message="Email sent successfully")
                print('\n Email sent Successfully')
            else:
                pass

        except:
            tkinter.messagebox.showerror(title="Unsuccessful", message="Please check your internet connection")

    frame4.mainloop()


def submit(e2,bloodgroup,e4,e5,e6,e7,e8):
    name = e2.get().upper().strip()
    bloodgroup = bloodgroup.get()
    date = e4.get().strip()
    dob = e5.get().strip()
    email = e6.get().upper().strip()
    contact = e7.get().strip()
    #if not(contact.index('+91 ')==0 and contact[4:].isdigit()):
     #   tkinter.messagebox.showerror('Please enter valid mobile number')
    #else:
     #   pass
    address = e8.get().upper().strip()

    if name and bloodgroup and date and dob and email and len(contact)==14 and address:
        con = sqlite3.connect("Blood_Bank")
        con.execute("create table if not exists Blood_Bank_Data(Name char[30], Blood_Group char[3],Date char[10],\
                     Date_Of_Birth char[10],Email char[50] unique,Contact_No char[14],Address char[50])")
        query = f"insert into Blood_Bank_Data(Name,Blood_Group,Date,Date_Of_Birth,Email,Contact_No,Address) \
                values('{name}','{bloodgroup}','{date}','{dob}','{email}','{contact}','{address}')"
        #query = "delete from Blood_Bank_Data where Blood_Group = 'PY_VAR6' "  '''
        #query = "delete from Blood_Bank_Data where Name = 'HARI'"
        con.execute(query)
        con.commit()
        tkinter.messagebox.showinfo(title="Successful",message="You Successfully Registered")
        print("Successfully Registered \n")
        m = con.execute("select * from Blood_Bank_Data")
        for row in m:
            print(row)
        #print(list(m))

    else:
        tkinter.messagebox.showerror(title = "Error", message = 'Please Fill All Required Fields')

home()

'''
def getdate(frame2):
    cd = CalendarDialog(frame2)
    result = cd.result
    selected_date.set(result.strftime('%d-%m-%Y'))
'''
# query = "delete from Blood_Bank_Data where Blood_Group = 'PY_VAR6' "
# kanikvijay9@gmail.com
#list(con.execute(f"select Email from Blood_Bank_Data where Blood_Group = '{var}'"))
