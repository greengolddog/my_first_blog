from flask import Flask, render_template, flash, request
from wtforms import Form, TextField, TextAreaField, validators, StringField, SubmitField

app = Flask(__name__)


class MyForm(Form):  # этот класс нужен, чтобы определить, как будет выглядеть форма
    name = TextField('число:')


def file(word):
    s = set()
    f = open(word)
    for line in f:
        s.add(line[:len(line) - 1])
    return s


def solve(s, voc):
    processed = [s]
    for c in voc:
        new_processed = []
        for i in range(0, 10):
            for p in processed:
                if str(i) not in p:
                    new_processed.append(p.replace(c, str(i)))
        processed = new_processed
    ret = []
    g = 0
    for p in processed:
        for i in range(0,len(p) - 1):
            if p[i] == '0':
                if (not (p[i - 1] in {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'})) or (i == 0):
                    if p[i + 1] in {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'}:
                        g = 1
                        break
            print(i)
        if g != 1 :
            if eval(p):
                ret.append(p)
        g = 0
    return ret


def solved(s, voc):
    processed = [s]
    for c in voc:
        new_processed = []
        for i in range(ord('a'), ord('z') + 1):
            for p in processed:
                if chr(i) not in p:
                    new_processed.append(p.replace(c, chr(i)))
        processed = new_processed
    word = 'ENRUS.txt'
    words = file(word)
    # print(words)
    ret = []
    d = 0
    for i in range(0, len(processed)):
        # print(processed[i])
        p = processed[i]
        q = p
        for b in range(0, len(q) - 1):
            # print(b,len(q))
            if p[b] in {"+", "=", '-', '*', '/', '(', ')'}:
                # print(p[:b],b,p[b:])
                p = p[:b] + ' ' + p[b + 1:]
                # print(p)
        ter = p.split()
        # print(ter)
        for c in ter:
            if not (c in words):
                d = 1
                break
        if d == 0:
            ret.append(processed[i])
        d = 0
    return ret


@app.route("/", methods=['GET', 'POST'])
def hello():
    form = MyForm(request.form)
    name = ''
    voc = []
    name2 = []
    if request.method == 'POST':  # запускается только если пользователь отправляет форму
        name = request.form['name']
        rot = request.form['drink']
        if rot == 'rad1':
            # print(1)
            voc = set(name) - {"+", "=", '-', '*', '/', '%', ' ', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '(', ')'}
            z = len(name) - 1
            i = 0
            # print(1)
            while i <= z:
                # print(1)
                if name[i] == '=' and name[i - 1] != '=' and name[i + 1] != '=':
                    # print(name)
                    name = name[:i] + '=' + name[i:]
                    z = z + 1
                    # print(name)
                i = i + 1
            # print(1)
            name2 = solve(name, voc)
            print(name2)
            # print(1.5+1.5)
        if rot == 'rad2':
            voc = set(name) - {"+", "=", '-', '*', '/', '%', ' ', '(', ')'}
            # print(voc)
            z = len(name) - 1
            i = 0
            while i <= z:
                if name[i] == '=' and name[i - 1] != '=' and name[i + 1] != '=':
                    print(name)
                    name = name[:i] + '=' + name[i:]
                    z = z + 1
                    print(name)
                i = i + 1
            if not eval(name):
                name2 = 'error'
            else:
                name2 = (solved(name, voc))
            print(name2)
    return render_template('main.html', form=form, name=name2)


if __name__ == "__main__":
    # if(...):
    #   raise ValueError("Заданный аргумент не является корректным ребусом")
    # print(int('078'))
    app.run()
