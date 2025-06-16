C + OpenGL 실습 결과 보고서

1번. 무지개 사각형 그리기

<설명>
이 코드는 OpenGL의 2D 렌더링 기능을 활용하여 색상이 자연스럽게 변하는 무지개 사각형을 그리는 프로그램입니다.
glBegin(GL_QUADS)와 glVertex2f() 함수를 이용해 사각형의 네 꼭짓점을 지정하고, 각 꼭짓점마다 서로 다른 색을 설정하여 OpenGL의 색상 보간(Gradient Interpolation) 기능을 이용해 무지개처럼 부드럽게 색이 연결되도록 구현했습니다.

실행 시, 500x500 크기의 윈도우에 중앙에 무지개 색 사각형이 표시되며, GLUT의 기본 이벤트 루프를 통해 화면을 유지합니다. 이 예제를 통해 OpenGL의 색상 처리와 기본 2D 도형 렌더링 방식을 이해할 수 있습니다.

<코드>
```c
#include <GL/freeglut.h>

void display() {
    glClear(GL_COLOR_BUFFER_BIT);

    glBegin(GL_QUADS);
    glColor3f(1.0, 0.0, 0.0); glVertex2f(-0.5f, -0.5f); // 빨강
    glColor3f(1.0, 1.0, 0.0); glVertex2f(0.5f, -0.5f);  // 노랑
    glColor3f(0.0, 1.0, 0.0); glVertex2f(0.5f, 0.5f);   // 초록
    glColor3f(0.0, 0.0, 1.0); glVertex2f(-0.5f, 0.5f);  // 파랑
    glEnd();

    glFlush();
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutCreateWindow("무지개 사각형");
    glutInitWindowSize(500, 500);
    glutDisplayFunc(display);
    glutMainLoop();
    return 0;
}

2번 회전하는 주전자

<설명>
이 코드는 OpenGL과 freeglut을 이용해 3D 객체인 와이어프레임 주전자를 Y축 기준으로 계속 회전시키는 애니메이션을 구현한 프로그램입니다.
glutWireTeapot() 함수는 freeglut에서 제공하는 3D 주전자 모델을 와이어프레임 형태로 출력해주며, glRotatef()를 통해 객체를 회전시키고, glutIdleFunc()을 통해 매 프레임마다 각도를 변경하여 부드러운 회전 애니메이션을 구현합니다.

깊이 테스트(glEnable(GL_DEPTH_TEST))와 더블 버퍼링(GLUT_DOUBLE)을 설정하여 3D 공간에서 물체가 자연스럽게 겹치도록 처리하고, 화면 깜빡임 없이 부드럽게 동작하도록 했습니다. 이 예제를 통해 OpenGL의 3D 객체 렌더링, 회전 변환, 애니메이션 처리 방식을 익힐 수 있습니다.

<코드>
```c
#include <GL/freeglut.h>

float angle = 0.0f;

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glLoadIdentity();

    glRotatef(angle, 0.0f, 1.0f, 0.0f);  // Y축 회전
    glutWireTeapot(0.5);                // 와이어프레임 주전자

    glutSwapBuffers();
}

void idle() {
    angle += 0.2f;
    if (angle > 360.0f) angle -= 360.0f;
    glutPostRedisplay();
}

void init() {
    glEnable(GL_DEPTH_TEST);
    glClearColor(0.1f, 0.1f, 0.1f, 1.0f);  // 배경 어두운 회색
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
    glutInitWindowSize(600, 600);
    glutCreateWindow("회전하는 주전자");

    init();
    glutDisplayFunc(display);
    glutIdleFunc(idle);
    glutMainLoop();
    return 0;
}
