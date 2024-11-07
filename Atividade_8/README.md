Marcelo Kodaira de Almeida - 10334750

# 1- Eexibir um triângulo com cor sólida na tela utilizando a API Vulkan

1. Inicialização da Instância Vulkan:

Criação da Instância: Inicie criando uma instância Vulkan (VkInstance), que serve como o ponto de entrada para a aplicação interagir com a API.
Seleção de Extensões e Camadas de Validação: Determine as extensões necessárias e, durante o desenvolvimento, ative camadas de validação para facilitar a depuração.
2. Configuração da Superfície de Apresentação:

Criação da Janela: Utilize uma biblioteca como GLFW ou SDL para criar uma janela que receberá a renderização.
Criação da Superfície: Crie uma superfície de apresentação (VkSurfaceKHR) que conecta a instância Vulkan à janela criada.
3. Seleção do Dispositivo Físico e Criação do Dispositivo Lógico:

Seleção do Dispositivo Físico: Escolha uma GPU adequada que suporte as operações desejadas.
Criação do Dispositivo Lógico: Crie um dispositivo lógico (VkDevice) a partir do dispositivo físico selecionado, especificando as filas necessárias, como a fila de gráficos.
4. Criação das Filas de Comando:

Obtenção das Filas: Recupere as filas de comando (VkQueue) do dispositivo lógico para enviar comandos de renderização.
5. Configuração do Swap Chain:

Criação do Swap Chain: Configure o swap chain (VkSwapchainKHR), que gerencia as imagens de apresentação.
Obtenção das Imagens do Swap Chain: Recupere as imagens associadas ao swap chain para uso na renderização.
6. Criação das Imagens e Views de Imagem:

Criação das Views de Imagem: Para cada imagem do swap chain, crie uma view de imagem (VkImageView) que define como a imagem será acessada.
7. Criação do Render Pass:

Definição do Render Pass: Estabeleça um render pass (VkRenderPass) que descreve as operações de renderização e os attachments utilizados.
8. Criação dos Framebuffers:

Criação dos Framebuffers: Para cada view de imagem, crie um framebuffer (VkFramebuffer) que associa o render pass às views de imagem correspondentes.
9. Criação dos Shaders:

Compilação dos Shaders: Escreva e compile os shaders (vertex e fragment) para o formato SPIR-V.
Criação dos Módulos de Shader: Crie módulos de shader (VkShaderModule) a partir dos códigos SPIR-V compilados.
10. Configuração do Pipeline Gráfico:

Definição do Pipeline: Configure o pipeline gráfico (VkPipeline), especificando os shaders, layout dos vértices, estado de rasterização, entre outros.
11. Criação dos Buffers de Vértices:

Definição dos Dados dos Vértices: Defina os dados dos vértices que compõem o triângulo.
Criação e Preenchimento dos Buffers: Crie buffers de vértices (VkBuffer) e copie os dados dos vértices para a memória apropriada.
12. Criação dos Buffers de Comando:

Alocação dos Buffers de Comando: Aloque buffers de comando (VkCommandBuffer) para armazenar os comandos de renderização.
Gravação dos Comandos: Grave os comandos de renderização nos buffers de comando, incluindo a vinculação do pipeline, buffers de vértices e a chamada de desenho (vkCmdDraw).
13. Configuração da Sincronização:

Criação de Semáforos e Fences: Crie semáforos e fences para sincronizar a apresentação e a renderização.
14. Loop de Renderização:

Aquisição da Imagem do Swap Chain: Adquira uma imagem disponível do swap chain para renderizar.
Submissão dos Buffers de Comando: Submeta os buffers de comando para execução na fila de gráficos.
Apresentação da Imagem: Apresente a imagem renderizada na janela.
15. Limpeza dos Recursos:

Destruição dos Recursos: Após o término da aplicação, destrua todos os recursos alocados, incluindo dispositivos, buffers, pipelines e o swap chain.


# 2- Um exemplo de código que demonstra o uso da API para exibir um triângulo com cór sólida na tela.

#include <vulkan/vulkan.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <stdexcept>
#include <cstdlib>
#include <vector>
#include <cstring>

const int WIDTH = 800;
const int HEIGHT = 600;

class HelloTriangleApplication {
public:
    void run() {
        initWindow();
        initVulkan();
        mainLoop();
        cleanup();
    }

private:
    GLFWwindow* window;
    VkInstance instance;

    void initWindow() {
        glfwInit();

        glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);

        window = glfwCreateWindow(WIDTH, HEIGHT, "Triângulo Vulkan", nullptr, nullptr);
    }

    void initVulkan() {
        createInstance();
        // Outros métodos de inicialização serão adicionados aqui
    }

    void mainLoop() {
        while (!glfwWindowShouldClose(window)) {
            glfwPollEvents();
            // Função de desenho será chamada aqui
        }
    }

    void cleanup() {
        vkDestroyInstance(instance, nullptr);

        glfwDestroyWindow(window);

        glfwTerminate();
    }

    void createInstance() {
        VkApplicationInfo appInfo{};
        appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO;
        appInfo.pApplicationName = "Hello Triangle";
        appInfo.applicationVersion = VK_MAKE_VERSION(1, 0, 0);
        appInfo.pEngineName = "Sem Motor";
        appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
        appInfo.apiVersion = VK_API_VERSION_1_0;

        VkInstanceCreateInfo createInfo{};
        createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
        createInfo.pApplicationInfo = &appInfo;

        // Obter extensões necessárias
        uint32_t glfwExtensionCount = 0;
        const char** glfwExtensions;

        glfwExtensions = glfwGetRequiredInstanceExtensions(&glfwExtensionCount);

        createInfo.enabledExtensionCount = glfwExtensionCount;
        createInfo.ppEnabledExtensionNames = glfwExtensions;

        createInfo.enabledLayerCount = 0;

        if (vkCreateInstance(&createInfo, nullptr, &instance) != VK_SUCCESS) {
            throw std::runtime_error("Falha ao criar instância Vulkan!");
        }
    }
};

int main() {
    HelloTriangleApplication app;

    try {
        app.run();
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
        return EXIT_FAILURE;
    }

    return EXIT_SUCCESS;
}



# 3 - Execução Shaders

1. Escrita dos Shaders:

Vertex Shader: Define a posição dos vértices e outras propriedades.
Fragment Shader: Define a cor dos fragmentos (pixels) gerados pelos vértices.
2. Compilação para SPIR-V:

Compilação: Utilize ferramentas como o glslangValidator para compilar os shaders escritos em GLSL para o formato SPIR-V, que é o formato intermediário utilizado pelo Vulkan.

3. Criação dos Módulos de Shader:

Criação dos Módulos: Utilize a função vkCreateShaderModule para criar módulos de shader (VkShaderModule) a partir dos códigos SPIR-V compilados.
4. Configuração do Pipeline Gráfico:

Definição do Pipeline: Configure o pipeline gráfico (VkPipeline), especificando os módulos de shader, layout dos vértices, estado de rasterização, entre outros.
5. Vinculação dos Shaders no Pipeline:

Associação dos Shaders: Durante a criação do pipeline, associe os módulos de shader às etapas correspondentes (vertex e fragment) utilizando estruturas como VkPipelineShaderStageCreateInfo.
6. Execução dos Shaders:

Renderização: Durante a submissão dos comandos de renderização, os shaders serão executados conforme definidos no pipeline, processando os dados dos vértices e gerando os fragmentos com as cores apropriadas.

# 4 - Vertex shader e fragment/pixel shader suportado pela API


-Vertex Shader (GLSL):
#version 450

layout(location = 0) in vec2 inPosition;
layout(location = 1) in vec3 inColor;

layout(location = 0) out vec3 fragColor;

void main() {
    gl_Position = vec4(inPosition, 0.0, 1.0);
    fragColor = inColor;
}


- Fragment Shader (GLSL):
#version 450

layout(location = 0) in vec3 fragColor;

layout(location = 0) out vec4 outColor;

void main() {
    outColor = vec4(fragColor, 1.0);
}


Fontes
Vulkan Documentation Project: 
VULKAN DOCS
Vulkan Guide: 
VULKAN DOCS
Vulkan Programming Guide: 
KHRONOS
Vulkan Specification: 
KHRONOS REGISTRY
Repositório Vulkan-Docs no GitHub: 
GITHUB
Site Oficial do Vulkan: