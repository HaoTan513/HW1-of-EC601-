# HW1-of-EC601-
Homework 1 of EC601. Team member: Hao Tan, Prateek Mehta.

#!/usr/bin/env python

'''
browse.py
=========

Sample shows how to implement a simple hi resolution image navigation

Usage
-----
browse.py [image filename]

'''

# Python 2/3 compatibility
from __future__ import print_function
import sys
PY3 = sys.version_info[0] == 3

if PY3:
    xrange = range

import numpy as np
import cv2

# built-in modules
import sys

if __name__ == '__main__':
    print('This sample shows how to implement a simple hi resolution image navigation. And shows how to browse image')
    print('USAGE: browse.py [image filename]')
    print()

    if len(sys.argv) > 1:
        fn = sys.argv[1]
        print('loading %s ...' % fn)
        img = cv2.imread(fn)  # read the img file
        if img is None:
            print('Failed to load fn:', fn)
            sys.exit(1)

    else:
        sz = 4096
        print('generating %dx%d procedural image ...' % (sz, sz))    # set the resolution of img as 4096*4096
        img = np.zeros((sz, sz), np.uint8)
        track = np.cumsum(np.random.rand(500000, 2)-0.5, axis=0)
        track = np.int32(track*10 + (sz/2, sz/2))
        cv2.polylines(img, [track], 0, 255, 1, cv2.LINE_AA)


    small = img
    for i in xrange(3):
        small = cv2.pyrDown(small)

    def onmouse(event, x, y, flags, param):
        h, w = img.shape[:2]     # h = img.shape[0], w = img.shape[1]
        h1, w1 = small.shape[:2]     # h1 = small.shape[0], w1 = small.shape[1]
        x, y = 1.0*x*h/h1, 1.0*y*h/h1
        zoom = cv2.getRectSubPix(img, (800, 600), (x+0.5, y+0.5))
        cv2.imshow('zoom', zoom)

    cv2.imshow('preview', small)     # print the final edition of img
    cv2.setMouseCallback('preview', onmouse)
    cv2.waitKey()
    cv2.destroyAllWindows()

