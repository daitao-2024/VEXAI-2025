步骤1：下载Anaconda安装程序 。\
打开浏览器，访问Anaconda官网。 点击“Download”，选择适合macOS的安装程序，然后点击“Download”按钮下载Anaconda安装程序。\
步骤2：运行安装程序 打开下载的安装程序文件（例如：Anaconda3-2022.05-MacOSX-x86_64.pkg）。 运行安装程序，按照提示完成安装过程。\
步骤3：配置Anaconda 打开“终端”（Terminal）。 安装完成后，Anaconda的安装目录通常会自动添加到PATH环境变量中。你可以通过运行以下命令验证Anaconda是否已正确安装：\
conda --version 你应该会看到类似 conda 4.10.1 的输出，表示Conda已成功安装。\
步骤4：配置清华大学镜像源（可选） 如果你在中国大陆，使用清华大学的镜像源可以加快安装速度：\
在终端中运行以下命令，配置Conda使用清华大学镜像源：\
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ \
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ \
conda config --set show_channel_urls yes \
配置完成后，可以使用以下命令验证：
conda config --show 你应该会看到已配置的清华大学镜像源。\
步骤5：更新Conda \
conda update conda \
步骤6：创建虚拟环境并安装PyTorch 创建虚拟环境： \
conda create -n pytorch_env python=3.8 \
以上命令将创建一个名为pytorch_env的虚拟环境，并安装Python 3.8。你可以根据需要调整Python版本。\
激活虚拟环境：\
conda activate pytorch_env \
安装PyTorch： 首先，确认你的CUDA版本。然后根据PyTorch官网提供的安装命令来安装GPU版本。\
以下是一个示例（假设你使用的是CUDA 11.1）：\
安装CPU版本的PyTorch \
conda install pytorch torchvision torchaudio cpuonly -c pytorch \
安装GPU版本的PyTorch（适用于具有NVIDIA GPU的系统） 如果你的macOS系统上有NVIDIA GPU并且你安装了CUDA，你可以使用以下命令：\
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c nvidia \
请根据你的CUDA版本调整cudatoolkit=11.1部分，例如cudatoolkit=10.2表示CUDA 10.2。\
步骤7：验证安装 安装完成后，验证PyTorch是否安装成功。运行以下命令来启动Python交互式环境：\
python \
然后在Python交互式环境中运行以下代码：\
import torch\
print("PyTorch version:", torch.**version**) # 检查PyTorch版本\
print("CUDA available:", torch.cuda.is_available())# 检查CUDA是否可用\
if torch.cuda.is_available(): print("GPU device name:", torch.cuda.get_device_name(0)) # 如果CUDA可用，打印GPU信息 \
如果输出PyTorch的版本号和相关信息，说明安装成功。\
通过以上步骤，你可以在macOS上成功安装Anaconda和PyTorch。这种方法不仅能加快下载速度，还能保证安装过程的稳定性。如果遇到任何问题，可以参考Anaconda的文档或PyTorch的安装指南。\
安装VScode：\
1. 访问清华大学开源软件镜像站
清华大学开源软件镜像站提供了Visual Studio Code的镜像。你可以通过以下链接访问：
https://mirrors.tuna.tsinghua.edu.cn/VSCode/ \

2. 下载Visual Studio Code\
打开浏览器，访问清华大学开源软件镜像站.\
选择适用于macOS的版本，一般选择最新的 .zip 文件，例如 VSCode-darwin-stable.zip。\
下载完成后，打开下载的 .zip 文件。\
将“Visual Studio Code”拖动到“Applications”文件夹中。\
3. 启动Visual Studio Code \
.打开“Applications”文件夹，找到“Visual Studio Code”，双击启动。\
.打开一个终端，输入 code命令启动。\
4. 安装完成后的配置\
   安装扩展：\
启动VS Code后，可以点击左侧栏的“Extensions”图标，搜索并安装你需要的扩展，如Python、JavaScript、C++等语言支持扩展。\
5. 配置用户设置：\
点击左下角的齿轮图标，然后选择“Settings”，可以根据需要进行配置\