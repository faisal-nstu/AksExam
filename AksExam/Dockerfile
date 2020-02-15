#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

#
# Angular Client build
#
FROM node:12 as angular-build
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["AksExam/AksExam.csproj", "AksExam/"]
RUN dotnet restore "AksExam/AksExam.csproj"
COPY . .
WORKDIR "/src/AksExam"
RUN dotnet build "AksExam.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AksExam.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AksExam.dll"]