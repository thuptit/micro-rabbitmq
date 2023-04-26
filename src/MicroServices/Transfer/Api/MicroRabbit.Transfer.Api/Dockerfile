#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR ./src
COPY ["Microservices/Transfer/Api/MicroRabbit.Transfer.Api/MicroRabbit.Transfer.Api.csproj", "Microservices/Transfer/Api/MicroRabbit.Transfer.Api/"]
COPY ["MicroRabbit.Infra.CrossCutting.IoC/MicroRabbit.Infra.IoC/MicroRabbit.Infra.CrossCutting.IoC.csproj", "MicroRabbit.Infra.CrossCutting.IoC/MicroRabbit.Infra.IoC/"]
COPY ["MicroRabbit.Infra.CrossCutting.Bus/MicroRabbit.Infra.Bus/MicroRabbit.Infra.CrossCutting.Bus.csproj", "MicroRabbit.Infra.CrossCutting.Bus/MicroRabbit.Infra.Bus/"]
COPY ["MicroRabbit.Domain.Core/MicroRabbit.Domain.Core/MicroRabbit.Domain.Core.csproj", "MicroRabbit.Domain.Core/MicroRabbit.Domain.Core/"]
COPY ["Microservices/Transfer/Application/MicroRabbit.Transfer.Application/MicroRabbit.Transfer.Application.csproj", "Microservices/Transfer/Application/MicroRabbit.Transfer.Application/"]
COPY ["Microservices/Transfer/Data/MicroRabbit.Transfer.Data/MicroRabbit.Transfer.Data.csproj", "Microservices/Transfer/Data/MicroRabbit.Transfer.Data/"]
COPY ["Microservices/Transfer/Domain/MicroRabbit.Transfer.Domain/MicroRabbit.Transfer.Domain.csproj", "Microservices/Transfer/Domain/MicroRabbit.Transfer.Domain/"]
RUN dotnet restore "Microservices/Transfer/Api/MicroRabbit.Transfer.Api/MicroRabbit.Transfer.Api.csproj"
COPY . .
WORKDIR "/src/Microservices/Transfer/Api/MicroRabbit.Transfer.Api"
RUN dotnet build "MicroRabbit.Transfer.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MicroRabbit.Transfer.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MicroRabbit.Transfer.Api.dll"]