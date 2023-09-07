# GPIO library
import Jetson.GPIO as GPIO
import pygame
import threading

# Handles time
import time 
GPIO.cleanup()
# Pin Definition
red_pin = 38
green_pin = 40

button_pin = 33
button_gnd = 29


GPIO.setmode(GPIO.BOARD)
GPIO.setup(red_pin, GPIO.OUT, initial=GPIO.LOW) 
GPIO.setup(green_pin, GPIO.OUT, initial=GPIO.LOW)  
GPIO.setup(button_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(button_gnd, GPIO.OUT, initial=GPIO.HIGH)
# Set GPIO mode


def Red_outLed():
  # led off
  try:
    GPIO.output(red_pin, GPIO.LOW)
  except KeyboardInterrupt:
    GPIO.cleanup()

def Green_outLed():
  # led off
  try:
    GPIO.output(green_pin, GPIO.LOW)
  except KeyboardInterrupt:
    GPIO.cleanup()
def RedLed():
  try:
    Green_outLed()
    GPIO.output(red_pin, GPIO.HIGH) 
  except KeyboardInterrupt:
    GPIO.cleanup()

def GreenLed():
  try:
    Red_outLed()
    GPIO.output(green_pin, GPIO.HIGH) 
  except KeyboardInterrupt:
    GPIO.cleanup()

##############################################
global isSwitchValue
isSwitchValue = 0
def switch_handler(channel):
  isSwitchValue = 1
  print('s', isSwitchValue)
  print('=====================switch======================')
  return False


def switch_event():
  GPIO.add_event_detect(button_pin, GPIO.FALLING, callback=switch_handler, bouncetime=200)
  print('e ', isSwitchValue)
# def blinkLed(stop_event):
#     GPIO.setup(led_pin, GPIO.OUT, initial=GPIO.HIGH)  
#     outLed()
#     while not stop_event.is_set():
#         time.sleep(1)
#         GPIO.output(led_pin, GPIO.HIGH)
#         time.sleep(1)
#         GPIO.output(led_pin, GPIO.LOW)



==================================================================

import pygame
import time
import playsound
import os
import Jetson.GPIO as GPIO

# GPIO library

from assist.gpio.led_gpio import switch_event
from assist.gpio.led_gpio import isSwitchValue
from assist.gpio.led_gpio import switch_handler



def speak():
    pygame.init()
    pygame.mixer.music.load('/home/jetson/Desktop/LangChain-StoryBot-main/assist/wav/snow_white.wav')
    pygame.mixer.music.play()
    
    
    global isSwitchValue
    #print('while no = ', isSwitchValue)
    switch_event()
    running = True
    while running:
        #print('running = ',isSwitchValue)
        if isSwitchValue == 1:
            print(11111, isSwitchValue)
            pygame.mixer.music.stop()
            pygame.mixer.music.unload()
            pygame.quit()
            running = False
            isSwitchValue = 0
            return False
            
        """        if isSwitchValue == 11:
            print(11111, isSwitchValue)
            pygame.mixer.music.stop()
            pygame.mixer.music.unload()
            pygame.quit()
            print(11111, isSwitchValue)
            #isSwitchValue = True
            running = False
            return False
        #print(3113, isSwitchValue)
        """
    return False

# ==================================
# import pygame
# import time
# import playsound
# import os

# def speak():
#   pygame.init()
#   pygame.mixer.music.load('/home/jetson/Desktop/LangChain-StoryBot-main/mp3/story.mp3')
#   pygame.mixer.music.play()

#   running = True

#   while running:
#     for event in pygame.event.get():
#       if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
#         pygame.mixer.music.stop()
#         pygame.mixer.music.unload()
#         running = False
#         pygame.quit()
#         time.sleep(1)
#         return False
      
"""
def speak(self, text):
        tts = gTTS(text=text, lang=self.language)
        tts.save('/home/jetson/Desktop/LangChain-StoryBot-main/mp3/output.mp3')

        # playsound를 사용하여 오디오 재생
        playsound.playsound('/home/jetson/Desktop/LangChain-StoryBot-main/mp3/output.mp3', True)
        # 재생 후 오디오 파일 삭제
        os.remove('/home/jetson/Desktop/LangChain-StoryBot-main/mp3/output.mp3') 
"""
