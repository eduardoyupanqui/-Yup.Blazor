#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.
# docker build -t eduyupanqui/blazor-wasm:1.0 -f src/BlazorWebAssemblyStandalone/Dockerfile .
# docker run --rm -it -p 8000:80 eduyupanqui/blazor-wasm:1.0


FROM nginx:alpine AS base
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/BlazorWebAssemblyStandalone/BlazorWebAssemblyStandalone.csproj", "src/BlazorWebAssemblyStandalone/"]
RUN dotnet restore "./src/BlazorWebAssemblyStandalone/BlazorWebAssemblyStandalone.csproj"
COPY . .
WORKDIR "/src/src/BlazorWebAssemblyStandalone"
RUN dotnet build "./BlazorWebAssemblyStandalone.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BlazorWebAssemblyStandalone.csproj" -c $BUILD_CONFIGURATION -o /app/publish

FROM base AS final
COPY ./nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=publish /app/publish/wwwroot /usr/share/nginx/html