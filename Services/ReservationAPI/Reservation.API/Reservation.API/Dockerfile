FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Services/ReservationAPI/Reservation.API/Reservation.API/Reservation.API.csproj", "Services/ReservationAPI/Reservation.API/Reservation.API/"]
COPY ["Services/ReservationAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure/Reservation.API.Infrastructure.csproj", "Services/ReservationAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure/"]
COPY ["Services/ReservationAPI/Reservation.API.Models/Reservation.API.Models/Reservation.API.Models.csproj", "Services/ReservationAPI/Reservation.API.Models/Reservation.API.Models/"]
COPY ["Services/ReservationAPI/Reservation.API.Services/Reservation.API.Services/Reservation.API.Services.csproj", "Services/ReservationAPI/Reservation.API.Services/Reservation.API.Services/"]
RUN dotnet restore "Services/ReservationAPI/Reservation.API/Reservation.API/Reservation.API.csproj"
COPY . .
WORKDIR "/src/Services/ReservationAPI/Reservation.API/Reservation.API"
RUN dotnet build "Reservation.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Reservation.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Reservation.API.dll"]
