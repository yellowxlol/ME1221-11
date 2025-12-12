import cv2
from fer.fer import FER
import pygame  # ← 新增：播放器

# ==============================
# 初始化 FER
# ==============================
emotion_detector = FER()

# ==============================
# 初始化音乐播放器
# ==============================
pygame.mixer.init()

emotion_music = {
    "happy": "music/happy.mp3",
    "sad": "music/sad.mp3",
    "angry": "music/angry.mp3",
    "surprise": "music/surprise.mp3",
    "neutral": "music/neutral.mp3",
    "fear": "music/fear.mp3",
    "disgust": "music/disgust.mp3"
}

current_music = None

def play_music_for_emotion(emotion):
    global current_music

    if emotion not in emotion_music:
        return

    music_file = emotion_music[emotion]

    # 情绪没变化，不切歌
    if current_music == music_file:
        return

    try:
        pygame.mixer.music.load(music_file)
        pygame.mixer.music.play(-1)  # 循环播放
        current_music = music_file
        print(f"[INFO] Now playing: {music_file}")
    except Exception as e:
        print(f"⚠️ Cannot play {music_file}: {e}")


# ==============================
# 摄像头部分（你们原来的完全保留）
# ==============================
cap = cv2.VideoCapture(0);

if not cap.isOpened():
    print("Error: Can't open the camera.")
    exit()

while True:
    ret, frame = cap.read()

    if not ret:
        print("Error: Can't read the video frame.")
        break

    faces = emotion_detector.detect_emotions(frame)
    for face in faces:
        box = face['box']
        emotion = face['emotions']

        # 画框（原封不动）
        cv2.rectangle(frame,
                      (box[0], box[1]),
                      (box[0]+box[2], box[1]+box[3]),
                      (0, 255, 0), 2)

        # 找最大情绪（保留你们原来的写法）
        max_emotion = {'none': 0}
        for key in emotion:
            if emotion.get(key) > next(iter(max_emotion.values())):
                max_emotion.clear()
                max_emotion[key] = emotion.get(key)

        # 得到当前最大情绪
        current_emotion = next(iter(max_emotion.keys()))

        # 显示在画面上
        cv2.putText(frame, current_emotion,
                    (box[0], box[1]+box[3]+10),
                    cv2.FONT_HERSHEY_COMPLEX,
                    0.4, (255, 255, 255), 1)

        # =======================
        # 新增：根据情绪播放音乐
        # =======================
        play_music_for_emotion(current_emotion)

    cv2.imshow('My Camera', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

print("The program is closing...")
cap.release()
cv2.destroyAllWindows()
pygame.mixer.quit()
