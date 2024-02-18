# Troubleshooting
1. 正常打开gazebo后无法显示world，一片灰：
   可能是笔记本无独立显卡，需要运行一行export LIBGL_ALWAYS_SOFTWARE=1指定用CPU渲染就好了