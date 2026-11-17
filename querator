#!/usr/bin/python

import enum
import os
from os.path import isdir
import sys

case_sensitive = False

class episode:
    def __init__(self, name, ep, se):
        self.name = name
        self.ep = ep
        self.se = se

def wild(file, hint):
    file = file.strip()
    hint = hint.strip()

    if not case_sensitive:
        file = file.lower()
        hint = hint.lower()

    w_l = len(hint) - hint.find("*")
    starter = hint[:len(hint) - w_l]
    s_l = len(starter)
    # print(w_l, starter, s_l)

    for i in range(len(file) - w_l - s_l + 1):
        # print(file[i:s_l + i])
        
        if file[i:s_l + i] == starter:
            try:
                # print(s_l + i, w_l + i)
                return int(file[s_l + i: s_l + w_l + i])
            except:
                continue
                # return file[s_l + i : w_l + i]

def formatter(file, hint):
    hint = hint.lower().split("e:")
    hint[0] = hint[0][2:]
    hint_se = hint[0]
    hint_ep = hint[1]
    se = wild(file, hint_se)
    ep = wild(file, hint_ep)
    
    if se is None or ep is None:
        return None

    return episode(file, ep, se)
 
    

ghelp = {
        "-h" : "get help. -h{v} for a more indepth}",
         "-r" : "recirsively get files from subdirs",
         "-d" : "recursive depth",
         "-f" : "give format hints",
         "-c" : "case sensitive (default: insensitive)",

         "-hr": "`-r`: will only look into 1 more sub-directory unless stated otherwise by -d",
         "-hd": "`-d {n}`: specify max depth n to look into for movie files, intended use with -r ",
         "-hf": "`-f`: provide hints in `s:{} e:{}` format. * represent wildcard: `s:season***e:episode***`. will default to `s:S**e:E**"}

f_hint = "s:S**e:E**"

argv = sys.argv
argc = len(argv)
cwd = os.getcwd()

recurse: bool = False;
depth = 1
def helper():
    for g, f in ghelp.items():
        print(f"{g}\t| {f}")

valid_ext = ["mp4", "mkv", "avi", "mov", "wmv", "flv", "webm", "mpeg", "mpg", "m4v", "3gp", "3g2", "ts", "mts", "m2ts", "vob", "ogv", "divx", "asf", "mxf", "f4v", "rm", "rmvb", "yuv", "m2v", "mpv", "mpe", "mp2", "m2p", "m2t", "roq", "nsv", "amv", "mkv"]

validated: list[str] = []

def validate(files) -> None:
    for file in files:
        name_ext = file.name.split('.')
        if name_ext[-1] in valid_ext:
            validated.append(file.name)
files = []

with os.scandir(cwd) as entries:
    for entry in entries:
        if entry.is_file():
            files.append(entry)
        if recurse and entry.is_dir():
            pass

if argc > 1:
    if argv[1] == "-h":
        helper()
    if "-h" in argv[1] and argv[1] in ghelp.keys():
        print(f"{argv[1]}\t| {ghelp[argv[1]]}")

# validate(files)

# print(validated)

# formatter("classroom of the elite s04e01", "s:s**e:e**")

unsorted_validated = []
validate(files)

for test in validated:
    t: episode | None = formatter(test, f_hint)
    if t is None:
        print(f"{test} cannot be deciphered")
        continue
    unsorted_validated.append(t)
    print(f"{t.name} is ok")

# first sort by episodes, then season
sorted_validated = sorted(unsorted_validated, key = lambda x: x.ep)
sorted_validated = sorted(sorted_validated, key = lambda x: x.se)

print("\n\nFinalList:\n")
print('\n'.join(t.name for t in sorted_validated))

with open(f'{cwd}/queue.m3u', 'w') as f:
    f.writelines(cwd + '/' + l.name + '\n' for l in sorted_validated)
