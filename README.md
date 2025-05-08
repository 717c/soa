import pygame
import random
import sys
import os
 
# 初始化pygame
pygame.init()
 
# 屏幕设置
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("2D赛车游戏")
 
# 颜色定义
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
GRAY = (100, 100, 100)
YELLOW = (255, 255, 0)
 
# 加载资源函数
def load_image(name, scale_x=1, scale_y=None):
    fullname = os.path.join('assets', name)
    try:
        image = pygame.image.load(fullname)
    except:
        print(f"无法加载图像: {fullname}")
        raise SystemExit
    
    if scale_y is None:
        scale_y = scale_x
    image = pygame.transform.scale(image, (
        int(image.get_width() * scale_x),
        int(image.get_height() * scale_y)
    ))
    return image.convert_alpha()
 
# 赛车类
class Car:
    def __init__(self, x, y, image_name):
        self.image = load_image(image_name, 0.15)
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed = 0
        self.max_speed = 10
        self.acceleration = 0.5
        self.deceleration = 0.8
        self.turn_rate = 5
        self.angle = 0
        self.width = self.image.get_width()
        self.height = self.image.get_height()
        
        # 碰撞检测点（前后左右各一个）
        self.collision_points = [
            pygame.Rect(x - 10, y - 30, 20, 10),  # 前
            pygame.Rect(x - 10, y + 20, 20, 10),   # 后
            pygame.Rect(x - 30, y - 10, 10, 20),   # 左
            pygame.Rect(x + 20, y - 10, 10, 20)    # 右
        ]
