# CGV-LAB

Program 1: 

Implement Brenham’s line drawing algorithm for all types of slope.

#include <GL/glut.h>
#include <stdio.h>
int x1, y1, x2, y2;
void myInit() 
{
	glClear(GL_COLOR_BUFFER_BIT);
	glClearColor(0.0, 0.0, 0.0, 1.0);
	glMatrixMode(GL_PROJECTION);
	gluOrtho2D(0, 500, 0, 500);
}

void draw_pixel(int x, int y) 
{
	glBegin(GL_POINTS);
	glVertex2i(x, y);
	glEnd();
}

void draw_line(int x1, int x2, int y1, int y2)
{
	int dx, dy, i, e; int incx, incy, inc1, inc2;
	int x, y;
	dx = x2 - x1;
	dy = y2 - y1;
	if (dx < 0) dx = -dx;
	if (dy < 0) dy = -dy;
	incx = 1;
	if (x2 < x1) incx = -1;
	incy = 1;
	if (y2 < y1) incy = -1;
	x = x1; y = y1;
	if (dx > dy) 
{
		draw_pixel(x, y);
		e = 2 * dy - dx;
		inc1 = 2 * (dy - dx);
		inc2 = 2 * dy;
		for (i = 0; i < dx; i++) 
{
		if (e >= 0) 
{
				y += incy;
				e += inc1;
				}
			else
				e += inc2;
			x += incx;
		draw_pixel(x, y);
			}
	}
	else 
{
		draw_pixel(x, y);
		e = 2 * dx - dy;
		inc1 = 2 * (dx - dy);
		inc2 = 2 * dx;
		for (i = 0; i < dy; i++) 
{
			if (e >= 0) 
{
				x += incx;
				e += inc1;
				}
			else
				e += inc2;
				y += incy;
				draw_pixel(x, y);
			}
		}
}

void myDisplay() 
{
	draw_line(x1, x2, y1, y2);
	glFlush();
}

int main(int argc, char** argv) 
{
	printf("Enter (x1, y1, x2, y2)\n");
	scanf_s ("%d %d %d %d", &x1, &y1, &x2, &y2);
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(500, 500);
	glutInitWindowPosition(0, 0);
	glutCreateWindow("Bresenham's Line Drawing");
	myInit();
	glutDisplayFunc(myDisplay);
	glutMainLoop();
	return 0;
}




Program 2

Create and rotate a triangle about the origin and a fixed point.

#include<GL/glut.h> 
#include<stdio.h> 
int x, y; int rFlag = 0;

void draw_pixel(float x1, float y1)
{
	glColor3f(0.0, 0.0, 1.0);
	glPointSize(5.0);
	glBegin(GL_POINTS);
	glVertex2f(x1, y1);
	glEnd();
}

void triangle()
{
	glColor3f(1.0, 0.0, 0.0);
	glBegin(GL_POLYGON);
	glVertex2f(100, 100);
	glVertex2f(250, 400);
	glVertex2f(400, 100);
	glEnd();
}
float th = 0.0;
float trX = 0.0, trY = 0.0;
void display()
{
	glClear(GL_COLOR_BUFFER_BIT);
	glLoadIdentity();
	if (rFlag == 1) //Rotate Around origin 
	{
		trX = 0.0; trY = 0.0; th += 0.1; draw_pixel(0.0, 0.0);
	}
	if (rFlag == 2) //Rotate Around Fixed Point 
	{
		trX = x; trY = y; th += 0.1; draw_pixel(x, y);
	} glTranslatef(trX, trY, 0.0);
	glRotatef(th, 0.0, 0.0, 1.0);
	glTranslatef(-trX, -trY, 0.0);
	triangle();
	glutPostRedisplay();
	glutSwapBuffers();
}

void myInit()
{
	glClearColor(0.0, 0.0, 0.0, 1.0);
	glMatrixMode(GL_PROJECTION);

	glLoadIdentity();
	gluOrtho2D(-500.0, 500.0, -500.0, 500.0);
	glMatrixMode(GL_MODELVIEW);
}

void rotateMenu(int option)
{
	if (option == 1)
		rFlag = 1; if (option == 2)
		rFlag = 2; if (option == 3)
		rFlag = 3;
}

int main(int argc, char** argv)
{
	printf("Enter Fixed Points (x,y) for Roration: \n");
	scanf_s("%d %d", &x, &y);
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
	glutInitWindowSize(500, 500);
	glutInitWindowPosition(0, 0);
	glutCreateWindow("Create and Rotate Triangle");
	myInit();
	glutDisplayFunc(display);
	glutCreateMenu(rotateMenu);
	glutAddMenuEntry("Rotate around ORIGIN", 1);
	glutAddMenuEntry("Rotate around FIXED POINT", 2);
	glutAddMenuEntry("Stop Rotation", 3);
	glutAttachMenu(GLUT_RIGHT_BUTTON);
	glutMainLoop();
}




