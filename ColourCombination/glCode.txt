#include <gl/glut.h>
#include <math.h>
#include<bits/stdc++.h>

using namespace std;

struct Point {
	GLint x;
	GLint y;
};

int r=255,g=255,b=255;
int xcordinate=0,ycordinate;
float dRadius = 1;
float radius[3] = {
	0.0f,
	40.0f,
    80.0f
};

void init() {
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glPointSize(7.0f);
	gluOrtho2D(0.0f, 640.0f, 0.0f, 480.0f);
}

void mouse(int button, int state, int mousex, int mousey)
{
    if(button==GLUT_LEFT_BUTTON && state==GLUT_DOWN)
    {
        xcordinate = mousex;
        ycordinate = mousey;
        if(xcordinate<490 && xcordinate>=0)
            cout<<"Click on Red, Green, or Blue strip to change colour"<<endl;
        else if(xcordinate<=662 && xcordinate>=612) // for blue strip
            cout<<"Red: "<<r<<"     Green: "<<g<<"     Blue: "<<256-(ycordinate/2)<<endl;
        else if(xcordinate<612 && xcordinate>=562) // for green strip
            cout<<"Red: "<<r<<"     Green: "<<ycordinate/2<<"     Blue: "<<b<<endl;
        else if(xcordinate<562 && xcordinate>=512) // for red strip
            cout<<"Red: "<<256-(ycordinate/2)<<"     Green: "<<g<<"     Blue: "<<b<<endl;
    }
    glutPostRedisplay();
}

void draw_circle(Point pC, GLfloat radius)
{
	GLfloat step = 1/radius;
	GLfloat x, y;
	for(GLfloat theta = 0; theta <= 360; theta += step)
    {
		x = pC.x + (radius * cos(theta));
		y = pC.y + (radius * sin(theta));
		glVertex2i(x, y);
	}
}

void display(void)
{
	glClear(GL_COLOR_BUFFER_BIT);
	if(xcordinate<=662 && xcordinate>=612) // for blue strip
    {
        b=256-(ycordinate/2);
    }
    if(xcordinate<612 && xcordinate>=562) // for green strip
    {
        g=ycordinate/2;
    }
	if(xcordinate<562 && xcordinate>=512) // for red strip
    {
        r=256-(ycordinate/2);
    }
	glBegin(GL_POINTS);
        Point pCenter = {245, 360};  // for red circle
        glColor3ub(r, 0, 0);
		for(int i = 0; i < 3; i++)
        {
			draw_circle(pCenter, radius[i]);
			radius[i]+=dRadius;
			if(radius[i] > 180)
				radius[i] = 0;
		}
        pCenter = {105, 150};  // for green circle
        glColor3ub(0, g, 0);
		for(int i = 0; i < 3; i++)
		{
			draw_circle(pCenter, radius[i]);
			radius[i]+=dRadius;
			if(radius[i] > 180)
				radius[i] = 0;
		}

        pCenter = {375, 150};  // for blue circle
        glColor3ub(0, 0,b);
		for(int i = 0; i < 3; i++)
        {
			draw_circle(pCenter, radius[i]);
			radius[i]+=dRadius;
			if(radius[i] > 180)
				radius[i] = 0;
		}

        pCenter = {245, 220}; // for resultant of blue green and red circle
        glColor3ub(r, g,b);
		for(int i = 0; i < 3; i++)
        {
			draw_circle(pCenter, radius[i]);
			radius[i]+=dRadius;
			if(radius[i] > 180)
				radius[i] = 0;
		}
	glEnd();

    int width=50,edge=640;
	glBegin(GL_QUADS);  // blue strip
        glColor3ub(0,0,0);
        glVertex2d(edge,0);
        glVertex2d(edge-width,0);
        glColor3ub(0,0,255);
        glVertex2d(edge-width,512);
        glVertex2d(edge,512);
    glEnd();

    glBegin(GL_QUADS); // green strip
        glColor3ub(0,255,0);
        glVertex2d(edge-width,0);
        glVertex2d(edge-(2*width),0);
        glColor3ub(0,0,0);
        glVertex2d(edge-(2*width),512);
        glVertex2d(edge-width,512);
    glEnd();

    glBegin(GL_QUADS);  // red strip
        glColor3ub(0,0,0);
        glVertex2d(edge-(2*width),0);
        glVertex2d(edge-(3*width),0);
        glColor3ub(255,0,0);
        glVertex2d(edge-(3*width),512);
        glVertex2d(edge-(2*width),512);
    glEnd();

	glFlush();
}

void Timer(int value)
{
	glutTimerFunc(33, Timer, 0);
	glutPostRedisplay();
}

int main(int argc, char **argv)
{
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
	glutInitWindowPosition(500, 240);
	glutInitWindowSize(662,512);
	cout<<"INSTRUCTIONS"<<endl;
	cout<<"Make your desired colour using red blue and green primary colour and Have Fun !!"<<endl;
	cout<<"Click on red strip for red colour, the red strip goes from 255 to 0, top to bottom respectively"<<endl;
	cout<<"Click on green strip for Green Colour, the green strip goes from 0 to 255, top to bottom respectively"<<endl;
	cout<<"Click on blue strip for blue colour, the blue strip goes from 255 to 0, top to bottom respectively"<<endl;
	cout<<"Initial values of Red, Green, and Blue are 255, 255, and 255 respectively"<<endl;
	glutCreateWindow("Colour Combination Computer Graphics Project");

	glutDisplayFunc(display);
    glutMouseFunc(mouse);
	init();
	Timer(0);
	glutMainLoop();
	return 0;
}
