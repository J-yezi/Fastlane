default_platform(:ios)

GIT_URL = "https://github.com/J-yezi/Profile.git"
USERNAME = "449266619@qq.com"
APP_IDENTIFIER = "com.jesse.demos"
GIT_BRANCH = "developer"
SCHEME_NAME = "demos"

platform :ios do
# 证书管理员或者打包方法

  # 开发者中心生成证书，并且保存到github上，并且会下载cer和profile到本地
  lane :generate do
    generate_certificate(type: "development")
    generate_certificate(type: "adhoc")
    generate_certificate(type: "appstore")
  end

  # 下载或者更新cer和profile证书，并且安装到本地
  lane :load_all_certificate do
    match(git_url: GIT_URL, type: "development", readonly: true)
    match(git_url: GIT_URL, type: "adhoc", readonly: true)
    match(git_url: GIT_URL, type: "appstore", readonly: true)
  end

  lane :dev do
    create_before
    # 保证是最新的证书
    match(git_url: GIT_URL, type: "adhoc" , readonly: true)
    # 打包
    create_ipa(configuration: "Debug", export_method: "ad-hoc")
    # 上传fir
    upload_fir
  end

  lane :adhoc do
    create_before
    # 保证是最新的证书
    match(git_url: GIT_URL, type: "adhoc" , readonly: true)
    # 打包
    create_ipa(configuration: "Release", export_method: "ad-hoc")
    # 上传fir
    upload_fir
  end

  def create_before
    # 更新本地git
    #private_git_pull
    # 执行pod install
    #cocoapods
    # 更新build number
    #increment_build_number
  end

  def create_ipa(configuration:, export_method:)
    gym(
      scheme: SCHEME_NAME,
      # 指定打包方式，Release 或者 Debug
      configuration: configuration,
      # 指定打包所使用的输出方式，目前支持app-store, package, ad-hoc, enterprise, development
      export_method: export_method,
      #workspace: "#{SCHEME_NAME}.xcworkspace",
      # 是否清空以前的编译信息
      clean: true,
      # 隐藏没有必要的文件
      silent: true,
      output_name: "#{SCHEME_NAME}",
      output_directory: "./build",
    )
    puts("\n*************| 打包完成 |*************\n")
  end

  def upload_fir
    firim(firim_api_token: "643c5fd58c5f7dc3d66b88056fa7d282")
    puts("\n*************| 上传完成 |*************\n")
  end

  def generate_certificate(type:)
    match(git_url: GIT_URL, username: USERNAME, app_identifier: APP_IDENTIFIER, type: type)
  end

  def private_git_pull(branch: GIT_BRANCH)
    # 确保切换到developer分支
    ensure_git_branch(branch: branch)
    # 更新本地git
    git_pull
  end

# 开发人员方法

  # 下载或者更新证书，并不会新建证书
  lane :load do
    match(git_url: GIT_URL, type: "development", readonly: true)
  end

# 共用方法

  # 注册新设备，并且更新profile证书，并且更新到github上
  lane :register do |options|
    register_device(name: options[:name], udid: options[:udid])
    _register(type: "development")
    _register(type: "adhoc")
  end

  def _register(type:)
    match(type: type, git_url: GIT_URL, force_for_new_devices: true)
  end

  lane :abc do
    match(type: "development", force: true)
  end
end