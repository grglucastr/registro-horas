FROM maven:3.9.11-eclipse-temurin-25-alpine
ARG APP_JAR=target/*.jar
WORKDIR /app

# Baixar o "agent" do DataDog
RUN curl -Lo dd-java-agent.jar "https://dtdg.co/latest-java-tracer"


# Se você preferir que o workflow gere o JAR (mvn package no CI),
# comente as linhas abaixo e use apenas a cópia do JAR gerado pelo CI:
# COPY --from=build /workspace/target/*.jar /app/app.jar

# Caso o seu CI já rodou "mvn package" e gerou target/*.jar no contexto do build,
# o passo abaixo copia o jar diretamente do contexto (útil quando workflow roda mvn antes do docker build)
COPY ${APP_JAR} /app/app.jar

# Expor porta (ajuste se necessário)
EXPOSE 8080

# Variáveis de ambiente para exportar dados para o DataDog
ENV DD_SERVICE="api-registro-horas"
ENV DD_VERSION=1.0.2
ENV DD_AGENT_HOST="dd_agent"
ENV DD_TRACE_AGENT_PORT="8126"
env DD_LOGS_INJECTION=true
ENV DD_ENV="dev"

# Variáveis de ambiente com valores padrão (podem ser sobrescritas em tempo de execução)
ENV SPRING_DATASOURCE_URL="jdbc:postgresql://pg_server:5432/registro_horas"
ENV SPRING_DATASOURCE_USERNAME="registro_horas"
ENV SPRING_DATASOURCE_PASSWORD="registro_horas"

# Opções de JVM recomendadas (ajuste conforme necessidade)
ENV JAVA_OPTS="-Xms256m -Xmx512m -XX:+UseContainerSupport"
ENV JAVA_TOOL_OPTIONS="-javaagent:/app/dd-java-agent.jar"


# Comando para rodar o jar
ENTRYPOINT [ "/bin/sh", "-c", "exec java $JAVA_OPTS -jar /app/app.jar" ]