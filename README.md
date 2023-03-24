#import moudol nessery for the game
import random
import curses
#initializ the curses for screen
screen = curses.initscr()
#hide the mouse from screen
curses.curs_set(0)
#get max screen hight and width
screen_height, screen_width = screen.getmaxyx()
#creat the new window
window = curses.newwin(screen_height, screen_width, 0, 0)
#allow window to revice input from the keypord
window.keypad(1)
#set the delay for update the screen
window.timeout(100)
#set the x&y cordinait of the inailiz posion of snake head
snake_x = screen_width // 4
snake_y = screen_height // 2
#definde the initial position the snake body
snake = [
  [snake_y, snake_x],
  [snake_y, snake_x-1],
  [snake_y, snake_x-2]
]
#creat the food in middle window 
food = [screen_height // 2, screen_width // 2]
#add the food use by charter from curses moduel
window.addch(food[0], food[1], curses.ACS_PI)
#set the inslializ posion movement is reight
key = curses.KEY_RIGHT
#creat the game loap untile the gamer user or quite the game
while True:
#get the next key will by prosses by user
  next_key = window.getch()
#if user dosnt input any thing key remaints same else key will be set the new prosser key
  key = key if next_key == -1 else next_key
#check the snake call ded or itself
  if snake[0][0] in [0, screen_height] or snake[0][1] in [0, screen_width] or snake[0] in snake[1:]:
#if coledis close or quite game
    curses.endwin()
    quit()
#set the new position of the snake beased the deirecition
  new_head = [snake[0][0], snake[0][1]]
  if key == curses.KEY_DOWN:
    new_head[0] += 1
  if key == curses.KEY_UP:
    new_head[0] -= 1 
  if key == curses.KEY_RIGHT:
    new_head[1] += 1
  if key == curses.KEY_LEFT:
    new_head[1] -= 1  
#insert the new head to the firest position of snake liest
  snake.insert(0, new_head)
#check if snake eat the food
  if snake[0] == food:
    food = None
#while food is remove geneart new food in arendom place on screen
    while food is None:
      new_food = [
        random.randint(1, screen_height-1),
        random.randint(1, screen_width-1)
      ]
#set the food to new food if new food genearted is not is not in snake body and add in to screen 
      food = new_food if new_food not in snake else None
    window.addch(food[0], food[1], curses.ACS_PI)
#othe wise remove last geerat snake body
  else:
    tail = snake.pop()
    window.addch(tail[0], tail[1], ' ')
#updat the posion of the snake one the screen 
  window.addch(snake[0][0], snake[0][1], curses.ACS_CKBOARD)
  
  
    



