# pymerge
script to pull out some pdf file pages from a large / directory

#
# You need to install PyPDF2 for this to work, this is not in the Standard LIBs.
# sudo apt-get install python-pip, sudo pip install PyPDF2
# 
# Written by Alan Adebayo March 2016
# This script will output a pdf in the directory the working set of pdfs are located. For example if pdf location = '/home/bob/pdf', final.pdf will be located in '/home/bob/pdf'
# If any files don't have a page 5, the script will fail
# THIS SCRIPT WILL NOT OVERWRITE/REWRITE OVER FILES THAT EXIST.
# You will get "PdfReadWarning: Multiple definitions in dictionary " errors. This is ok.
# Tested with Python 2.7.6 (default, Jun 22 2015, 17:58:13)  [GCC 4.8.2] on linux2

import PyPDF2
import os
import time
def getPath():
    pdflocation = raw_input('Please give path: ')
    os.chdir(pdflocation)
    print(os.getcwd())

def listPDF():
    file_list = [ x for x in os.listdir(os.getcwd()) if x.endswith('.pdf')]
    print("list of files to be used, waiting 10s: ")
    print(file_list)
    time.sleep(10)
    return (file_list)

def pdfWork():
    outputname = raw_input('Please give output file name : ' ) + '.pdf'
    pdfm = PyPDF2.PdfFileMerger(strict=False) #opens PyPDF2 File Merger object
    for fname in listPDF(): # get the name of pdfs and appends pdf page (literal) number 5 to memory output
        input1= open(os.path.join(fname), "rb")
        print(input1)
        pdfm.append(fileobj = input1, bookmark=None, pages= (4,5), import_bookmarks = False) #'pages = (4,5)' meaning start at 4, stop at 5
    output = open(os.path.join(outputname), "wab") #name of the output pdf
    pdfm.write(output)
    pdfm.close()
def main():
    getPath()
    listPDF()
    pdfWork()

main()
