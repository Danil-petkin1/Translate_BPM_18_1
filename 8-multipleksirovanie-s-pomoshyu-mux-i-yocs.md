# 8.1 Настройка файлов запуска на использование тем mux

Пока всё хорошо, но как все эти темы и сообщения в конечном итоге контролируют базу роботов? До сих пор мы предполагали, что базовый контроллер робота прослушивает сообщения `Twist`в теме `/cmd_vel`. С запущенным выше `mux`-узлом, мы все еще можем контролировать базу таким образом: публикуя сообщения `Twist`непосредственно в теме `/cmd_vel`. Однако, для реализации функции мультиплексирования в `mux`-узле, мы должны настроить наш навигационный контроллер \(например, `move_base`\) на публикацию `Twist`-сообщений в теме `move_base_cmd_vel` и телеоп-узел джойстика на публикацию сообщений в теме `joystick_cmd_vel`. Таким образом, ни один из узлов не публикуется непосредственно на традиционной теме `/cmd_vel`; вместо этого, mux узел получает решение о том, какой вход будет передан в `/cmd_vel`.   
Пример того, как это сделать, вы можете найти в двух файлах запуска `mux_fake_move_base.launch` и `mux_joystick_teleop.launch` в каталоге `rbx2_nav/launch`. Сначала посмотрим на mux\_fake\_move\_base.launch:

```text
<launch>
 <node pkg="move_base" type="move_base" respawn="false" name="move_base"
clear_params="true" output="screen">
 <remap from="cmd_vel" to="move_base_cmd_vel" />
 <rosparam file="$(find rbx2_nav)/config/fake/costmap_common_params.yaml"
command="load" ns="global_costmap" />
 <rosparam file="$(find rbx2_nav)/config/fake/costmap_common_params.yaml"
command="load" ns="local_costmap" />
 <rosparam file="$(find rbx2_nav)/config/fake/local_costmap_params.yaml"
command="load" />
 <rosparam file="$(find rbx2_nav)/config/fake/global_costmap_params.yaml"
command="load" />
 <rosparam file="$(find rbx2_nav)/config/fake/base_local_planner_params.yaml"
command="load" />
 </node>
</launch>
```

Ключевая линия выделена жирным шрифтом выше. Эта строка просто возвращает тему `cmd_vel`, которую обычно публикует команда `move_base`, к теме `move_base_cmd_vel`. Вот и все. Файл `mux_joystick_teleop.launch` использует аналогичное переопределение:

```text
<launch>
 <!--- Teleop Joystick -->
 <node pkg="turtlebot_teleop" type="turtlebot_teleop_joy"
name="turtlebot_teleop_joystick" output="screen">
 Multiplexing Topics with mux & yocs - 229
 <remap from="turtlebot_teleop_joystick/cmd_vel" to="joystick_cmd_vel" />
 <param name="scale_angular" value="1.5"/>
 <param name="scale_linear" value="0.3"/>
 <param name="axis_deadman" value="7"/>
 </node>
 <node pkg="joy" type="joy_node" name="joystick" output="screen" />
</launch>
```

И снова, выделенная строка обеспечивает переадресацию, на этот раз с `turtlebot_teleop_joystick_cmd_vel`, которая используется по умолчанию узлом `turtlebot_teleop_joy`, который мы используем для темы `joystick_cmd_vel`, которую мы будем использовать с нашим `mux` узлом. \(Мы могли бы просто использовать название темы `turtlebot_teleop_joystick_cmd_vel` непосредственно с нашим `mux` узлом, но мы выбрали `joystick_cmd_vel`, чтобы он был более универсальным, и поэтому нам нужно вернуть его здесь\).

