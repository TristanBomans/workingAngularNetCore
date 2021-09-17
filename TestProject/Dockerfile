#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["TestProject.csproj", "."]
RUN dotnet restore "TestProject.csproj"
COPY . .
RUN echo 'yoo!'
RUN dotnet build "TestProject.csproj" -c Release -o /app/build

FROM node:10.15-alpine AS client 
WORKDIR /app 
COPY ClientApp . 
RUN npm install 
RUN npm run-script build

FROM build AS publish
RUN dotnet publish "TestProject.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
COPY --from=client /app/dist /app/dist
COPY --from=client /app/dist /wwwroot
ENTRYPOINT ["dotnet", "TestProject.dll"]