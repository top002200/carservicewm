# ---------- Build Stage ----------
FROM golang:1.23-alpine AS build

# ✅ ติดตั้งเครื่องมือที่จำเป็นสำหรับ CGO
RUN apk add --no-cache gcc g++ musl-dev

# ✅ เปิดใช้งาน CGO
ENV CGO_ENABLED=1

# ตั้ง working directory
WORKDIR /app

# คัดลอก go.mod และ go.sum ก่อน เพื่อ cache dependency
COPY go.mod go.sum ./

# ติดตั้ง dependency
RUN go mod download

# คัดลอก source code ทั้งหมดเข้ามา
COPY . .

# ✅ สร้าง binary ชื่อ main (พร้อม CGO)
RUN go build -o main .

# ---------- Run Stage ----------
FROM alpine:latest

# ✅ ติดตั้ง timezone และ sqlite-libs เพื่อให้ binary ใช้งานได้
RUN apk add --no-cache tzdata sqlite-libs

# ตั้ง working directory
WORKDIR /root/

# คัดลอก binary จาก build stage
COPY --from=build /app/main .

# Expose port 8000 ตามที่ main.go ใช้
EXPOSE 8000

# รันโปรแกรม
CMD ["./main"]
