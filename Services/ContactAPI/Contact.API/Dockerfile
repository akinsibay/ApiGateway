FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Services/ContactAPI/Contact.API/Contact.API.csproj", "Services/ContactAPI/Contact.API/"]
COPY ["Services/ContactAPI/Contact.API.Infrastructure/Contact.API.Infrastructure.csproj", "Services/ContactAPI/Contact.API.Infrastructure/"]
COPY ["Services/ContactAPI/Contact.API.Models/Contact.API.Models/Contact.API.Models.csproj", "Services/ContactAPI/Contact.API.Models/Contact.API.Models/"]
COPY ["Services/ContactAPI/Contact.API.Services/Contact.API.Services/Contact.API.Services.csproj", "Services/ContactAPI/Contact.API.Services/Contact.API.Services/"]
RUN dotnet restore "Services/ContactAPI/Contact.API/Contact.API.csproj"
COPY . .
WORKDIR "/src/Services/ContactAPI/Contact.API"
RUN dotnet build "Contact.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Contact.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Contact.API.dll"]
