FROM maven:3.9.5-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
# 実行権限を付与
RUN chmod +x mvnw 
RUN ./mvnw clean package -DskipTests

FROM eclipse-temurin:17-jdk
WORKDIR /app
# ビルドステージからjarをコピー
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]