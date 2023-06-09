#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR ./src
COPY ["MicroServices/Banking/Api/MicroRabbit.Banking.Api/MicroRabbit.Banking.Api.csproj", "MicroServices/Banking/Api/MicroRabbit.Banking.Api/"]
COPY ["MicroRabbit.Infra.CrossCutting.IoC/MicroRabbit.Infra.IoC/MicroRabbit.Infra.CrossCutting.IoC.csproj", "MicroRabbit.Infra.CrossCutting.IoC/MicroRabbit.Infra.IoC/"]
COPY ["MicroRabbit.Infra.CrossCutting.Bus/MicroRabbit.Infra.Bus/MicroRabbit.Infra.CrossCutting.Bus.csproj", "MicroRabbit.Infra.CrossCutting.Bus/MicroRabbit.Infra.Bus/"]
COPY ["MicroRabbit.Domain.Core/MicroRabbit.Domain.Core/MicroRabbit.Domain.Core.csproj", "MicroRabbit.Domain.Core/MicroRabbit.Domain.Core/"]
COPY ["Microservices/Banking/Application/MicroRabbit.Banking.Application/MicroRabbit.Banking.Application.csproj", "Microservices/Banking/Application/MicroRabbit.Banking.Application/"]
COPY ["Microservices/Banking/Domain/MicroRabbit.Banking.Domain/MicroRabbit.Banking.Domain.csproj", "Microservices/Banking/Domain/MicroRabbit.Banking.Domain/"]
COPY ["Microservices/Banking/Data/MicroRabbit.Banking.Data/MicroRabbit.Banking.Data.csproj", "Microservices/Banking/Data/MicroRabbit.Banking.Data/"]
RUN dotnet restore "MicroServices/Banking/Api/MicroRabbit.Banking.Api/MicroRabbit.Banking.Api.csproj"
COPY . .
WORKDIR "/src/MicroServices/Banking/Api/MicroRabbit.Banking.Api"
RUN dotnet build "MicroRabbit.Banking.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MicroRabbit.Banking.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MicroRabbit.Banking.Api.dll"]