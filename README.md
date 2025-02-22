import cv2

# بارگیری طبقهبندی Haar cascade برای تشخیص صورت
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

# روشن کردن دوربین سلفی (معمولاً شماره ۰ یا ۱)
cap = cv2.VideoCapture(0)  # اگر کار نکرد، شماره را به ۱ تغییر دهید

while True:
    # خواندن فریم از دوربین
    ret, frame = cap.read()
    if not ret:
        print("دسترسی به دوربین ممکن نیست.")
        break

    # تبدیل فریم به خاکستری برای پردازش سریعتر
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # تشخیص صورت در تصویر
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

    # کشیدن مستطیل سبز دور صورتهای تشخیص دادهشده
    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

    # نمایش خروجی
    cv2.imshow('تشخیص صورت', frame)

    # خروج با فشار دادن کلید q
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# آزاد کردن منابع
cap.release()
cv2.destroyAllWindows()
