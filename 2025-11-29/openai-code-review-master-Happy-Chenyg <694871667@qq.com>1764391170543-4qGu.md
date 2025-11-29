# Happy-Chenyg项目： OpenAi 代码评审.
### 😀代码评分：80
#### 😀代码逻辑与目的：
该代码片段是对OpenAi代码审查SDK的配置和实现进行更新。主要更改包括：更新代码审查日志的URL、添加新的代码审查SDK依赖、添加新的模型枚举和修改相关服务以使用新模型。

#### 🎯修改建议：
1. 修复代码审查日志URL的拼写错误。
2. 更新模型枚举，添加新的模型`GLM_4_5_FLASH`。
3. 更新相关服务使用新的模型`GLM_4_5_FLASH`。
4. 添加单元测试以验证新模型的正确使用。

#### 💻修改后的代码：
```yaml
diff --git a/.github/workflows/main-maven-jar.yml b/.github/workflows/main-maven-jar.yml
index bf9a61b..c23cb68 100644
--- a/.github/workflows/main-maven-jar.yml
+++ b/.github/workflows/main-maven-jar.yml
@@ -56,7 +56,7 @@ jobs:
       - name: Run Code Review
         run: java -jar ./libs/openai-code-review-sdk-1.0.jar
         env:
-          # Github 配置；GITHUB_REVIEW_LOG_URI「https://github.com/xfg-studio-project/openai-code-review-log」、GITHUB_TOKEN「https://github.com/settings/tokens」
+          # Github 配置；GITHUB_REVIEW_LOG_URI「https://github.com/Happy-Chenyg/openai-code-rewiew-log」、GITHUB_TOKEN「https://github.com/settings/tokens」
           GITHUB_REVIEW_LOG_URI: ${{ secrets.CODE_REVIEW_LOG_URI }}
           GITHUB_TOKEN: ${{ secrets.CODE_TOKEN }}
           COMMIT_PROJECT: ${{ env.REPO_NAME }}
diff --git a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/model/Model.java b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/model/Model.java
index 623d58f..c12751a 100644
--- a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/model/Model.java
+++ b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/model/Model.java
@@ -17,8 +17,10 @@ public enum Model {
     /** 智谱AI 24年01月发布 */
     GLM_3_5_TURBO("glm-3-turbo","适用于对知识量、推理能力、创造力要求较高的场景"),
     GLM_4("glm-4","适用于复杂的对话交互和深度内容创作设计的场景"),
-    GLM_4V("glm-4v","根据输入的自然语言指令和图像信息完成任务，推荐使用 SSE 或同步调用方式请求接口"),
-    GLM_4_FLASH("glm-4-flash","适用简单任务，速度最快，价格最实惠的版本，具有128k上下文"),
+    GLM_4V("glm-4v","根据输入的自然语言指令和图像信息完成任务,推荐使用 SSE 或同步调用方式请求接口"),
+    GLM_4_FLASH("glm-4-flash","适用简单任务,速度最快,价格最实惠的版本,具有128k上下文"),
+    /** 智谱AI 24年11月发布 - 完全免费 */
+    GLM_4_5_FLASH("glm-4.5-flash","完全免费的高性能模型,适用于简单任务,速度快,具有128k上下文"),
     COGVIEW_3("cogview-3","根据用户的文字描述生成图像,使用同步调用方式请求接口"),
     ;
     private final String code;
diff --git a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/service/impl/OpenAiCodeReviewService.java b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/service/impl/OpenAiCodeReviewService.java
index 7ecbde7..e6aab8e 100644
--- a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/service/impl/OpenAiCodeReviewService.java
index 7ecbde7..e6aab8e 100644
+++ b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/domain/service/impl/OpenAiCodeReviewService.java
@@ -31,7 +31,7 @@ public class OpenAiCodeReviewService extends AbstractOpenAiCodeReviewService {
     @Override
     protected String codeReview(String diffCode) throws Exception {
         ChatCompletionRequestDTO chatCompletionRequest = new ChatCompletionRequestDTO();
-        chatCompletionRequest.setModel(Model.GLM_4_FLASH.getCode());
+        chatCompletionRequest.setModel(Model.GLM_4_5_FLASH.getCode());
         chatCompletionRequest.setMessages(new ArrayList<ChatCompletionRequestDTO.Prompt>() {
             private static final long serialVersionUID = -7988151926241837899L;
 
@@ -52,7 +52,7 @@ public class OpenAiCodeReviewService extends AbstractOpenAiCodeReviewService {
                         "3. 不要携带变量内容解释信息。\n" +
                         "4. 有清晰的标题结构\n" +
                         "返回格式严格如下：\n" +
-                        "# 小傅哥项目： OpenAi 代码评审.\n" +
+                        "# Happy-Chenyg项目： OpenAi 代码评审.\n" +
                         "### \uD83D\uDE00代码评分：{变量1}\n" +
                         "#### \uD83D\uDE00代码逻辑与目的：\n" +
                         "{变量6}\n" +
diff --git a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/infrastructure/openai/dto/ChatCompletionRequestDTO.java b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/infrastructure/openai/dto/ChatCompletionRequestDTO.java
index 445c985..64b4e5e 100644
--- a/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/infrastructure/openai/dto/ChatCompletionRequestDTO.java
+++ b/openai-code-review-sdk/src/main/java/com/chenyg/middleware/sdk/infrastructure/openai/dto/ChatCompletionRequestDTO.java
@@ -8,7 +8,7 @@ import java.util.List;
 
 public class ChatCompletionRequestDTO {
 
-    private String model = Model.GLM_4_FLASH.getCode();
+    private String model = Model.GLM_4_5_FLASH.getCode();
     private List<Prompt> messages;
 
     public static class Prompt {
```