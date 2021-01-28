# PDFMerger

Funktion für die Erstellung einer GUI zum besseren Umgang mit der Anwendung.
```python
def openGUI():
    # Erstellt den "Hintergrund"
    root.title('PDF-Merge')
    root.geometry("300x160")
    root.resizable(False, False)
    # Text über dem Eingabe-Feld
    information_text = Label(root, text="Name des Output-Files")
    information_text.pack(side=TOP)
    # Eingabe-Feld für den Namen der PDF
    name_input = Entry(root, bd=5)
    name_input.pack(side=TOP)
    # Button, welcher die merge_pdfs() Funktion aufruft.
    b = Button(root, command=lambda: merge_pdfs(name_input.get() + ".pdf"), height=5, width=50, 
    text="PDFs auswählen.", bg="#378dae", activebackground="#3d9dc2")
    b.pack(pady=2)
    root.mainloop()


# Button, welcher später noch abgeändert wird, um den Benutzer über den Status des Programms zu informieren.
output = Label(root, text="", fg="white")
```


Funktion für das mergen mehrerer PDFs. Die PDFs werden einfach einander gehängt und in einer Datei ausgegeben.
```python
def merge_pdfs(result_name):
    try:
        # Stoppt die Funktion, wenn der User keinen Namen eingegeben hat.
        if result_name == ".pdf":
            change_btn("Keinen Namen eingegeben", "red")
            return
        pdf_writer = PdfFileWriter()
        files = filedialog.askopenfilenames(parent=root, title="Wähle die PDFs aus")

        for file in root.tk.splitlist(files):
            pdf_reader = PdfFileReader(file)
            for page in range(pdf_reader.getNumPages()):
                pdf_writer.addPage(pdf_reader.getPage(page))

        with open(result_name, 'wb') as out:
            # Informiert den User, ob die Dateien erfolgreich zusammengefügt wurden oder nicht.
            if pdf_reader.getNumPages() != 0:
                change_btn("Erfolgreich als " + result_name + " zusammengefügt!", "green")
            else:
                change_btn("Keine Dateien ausgewählt.", "red")
            pdf_writer.write(out)
    except:
        change_btn("Keine Dateien ausgewählt.", "red")
        # PDFs sind leer, wenn es einen Error gibt. Daher lösche ich sie mit der folgenden Zeile
        os.remove(result_name)
```

