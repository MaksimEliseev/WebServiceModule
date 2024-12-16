# Используем SDK для сборки
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

# Копируем файлы проектов
COPY ./WebServiceModule/WebServiceModule.csproj ./WebServiceModule/
COPY ./WebServiceModule/DataModule/DataModule.csproj ./DataModule/

# Восстанавливаем зависимости
RUN dotnet restore "WebServiceModule/WebServiceModule.csproj"

# Копируем весь исходный код
COPY ./WebServiceModule/ ./WebServiceModule/
COPY ./WebServiceModule/DataModule/ ./DataModule/

# Сборка проекта
WORKDIR /src/WebServiceModule
RUN dotnet build "WebServiceModule.csproj" -c Release -o /app/build

# Публикация
RUN dotnet publish "WebServiceModule.csproj" -c Release -o /app/publish

# Используем ASP.NET Runtime для запуска
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

# Запускаем приложение
ENTRYPOINT ["dotnet", "WebServiceModule.dll"]
