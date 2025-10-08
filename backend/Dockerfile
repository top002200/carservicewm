# ---------- Build Stage ----------
FROM golang:1.23-alpine AS build


# ตั้ง working directory
WORKDIR /app

# คัดลอก go.mod และ go.sum ก่อน เพื่อ cache dependency
COPY go.mod go.sum ./

# ติดตั้ง dependency
RUN go mod download

# คัดลอก source code ทั้งหมดเข้ามา
COPY . .

# สร้าง binary ชื่อ main
RUN go build -o main .

# ---------- Run Stage ----------
FROM alpine:latest

# ติดตั้ง timezone (optional)
RUN apk add --no-cache tzdata

# ตั้ง working directory
WORKDIR /root/

# คัดลอก binary จาก build stage
COPY --from=build /app/main .

# Expose port 8000 ตามที่ main.go ใช้
EXPOSE 8000

# รันโปรแกรม
CMD ["./main"]
